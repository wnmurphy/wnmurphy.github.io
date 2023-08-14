---
title: Creating a versatile multi-prompt chatbot with memory and a data store in LangChain.

date: 2023-08-13T07:00:00-08:00
layout: single
permalink: /creating-a-versatile-multi-prompt-chatbot-with-memory-and-a-data-store-in-langchain/
category: 'programming'
tags: ['langchain', 'llm', 'chatbot']
---

Here's an example approach to creating a chatbot with LangChain which integrates:

* A default conversation model.
* Multi-prompt routing so your chatbot can switch between modes depending on the topic.
* Router memory, so the routing model can tell if we're still on the same topic, making routes "stickier".
* A data store, so all chat messages persist.
* A chat memory, so each prediction is aware of the prior conversation.

This combination is a versatile foundational architecture, creating a chatbot experience which is robust and easily extensible.

In this example, we'll use PaLM's `chat-bison` as our default conversation model, and `text-bison` for all other operations.

This assumes you have LangChain installed, and are importing components as needed.

## Add models

Create the models:

```python
chat_model = ChatVertexAI(
        model_name=settings.CHAT_BISON,
        location="us-central1",
        project=settings.GCP_PROJECT,
        **model_params,
)

text_model = VertexAI(
        model_name=settings.TEXT_BISON,
        location="us-central1",
        project=settings.GCP_PROJECT,
        **model_params,
)
```


## Add prompt

Load our prompt template and create a `SystemMessagePromptTemplate` from it:

```python
prompt_template_yaml = get_prompt_template("chat_prompt_v1_1.yaml")
my_prompt_template = SystemMessagePromptTemplate.from_template(
        prompt_template_yaml,
        template_format="jinja2",
)
```

Inject our own contextual data:

```python
my_prompt_template = lc_prompt_template.format(**context_data)
```

Build the chat prompt. We'll append placeholders for the chat history (this will be injected automatically later when we add memory with a data store), and the user's incoming message:

```python
msg_templates = [
    my_prompt_template,
    MessagesPlaceholder(variable_name="chat_history"),
    HumanMessagePromptTemplate.from_template("{input}"),
]
default_chat_prompt = ChatPromptTemplate.from_messages(msg_templates)
```


## Add a data store

Create our data store. In this example, we're using Firestore:

```python
chat_history_obj = FirestoreChatMessageHistory(
    firestore_client=firestore_client,
    collection_name="ChatbotMessageHistory",
    user_id=body.get("user_id"),
)
```

Create the memory and hook up the data store to it.

```python
main_memory = ConversationBufferMemory(
    memory_key="chat_history",
    chat_memory=chat_history_obj,
    # Parse output as `HumanMessage`/`AIMessage` pairs. 
    return_messages=True,
)
```


## Add the default chain

Create a `ConversationChain`, and hook up the memory which now has a data store. This will be our router's default chain, which will make small talk our chatbot's default mode.

```python
default_chain = ConversationChain(
    llm=chat_model,
    prompt=default_chat_prompt,
    memory=main_memory,
    verbose=True,
    output_key="text",
)
```


## Add routing

Let's add routing. We'll use a `MultiPromptRouter` with an `LLMRouterChain` to decide which destination chain to route to, based on the incoming message, and the description for each of our destination chains. 

If none are a good match, it will just use the ConversationChain for small talk.


### Add router memory (topic awareness)

To make this routing "sticky" for a more natural experience, we'll add a `ConversationBufferWindowMemory` to make it aware of the current topic, rather than just the incoming message. This makes chatbot interactions feel much more natural.

```python
last_k_msgs_memory = ConversationBufferWindowMemory(
    memory_key="last_k_msgs",
    # Pass in the data store.
    chat_memory=chat_history_obj,
    # We'll inject these message directly as a string, no need to parse.
    return_messages=False,
    k=2,
)
```


### Add the router prompt

This is the prompt that the `LLMRouterChain` will use to choose a destination chain based on the incoming message.

We'll modify the default multi-prompt router prompt:
* Remove language allowing the model to modify the incoming message.
* Inject the last k most recent chat messages to achieve topic awareness.
* Add weight to the most recent chat messages when choosing a prompt.
* Add few-shot examples to improve performance.

````python
MULTI_PROMPT_ROUTER_TEMPLATE = """
Given a raw text input to a language model select the model prompt best suited for the input.
You will be given the names of the available prompts and a description of what the prompt is best suited for.
You will also be given the most recent chat messages.

Check the most recent chat messages to identify the current topic when selecting a model prompt.

When selecting a model prompt:
1. Base 80% of your decision on the recent chat messages.
2. Base 20% of your decision on the raw text.

<< FORMATTING >>
Return a markdown code snippet with a JSON object formatted to look like:
```json
{{{{
    "destination": string \\ name of the prompt to use or "DEFAULT"
    "next_inputs": string \\ the original input
}}}}
```
<< EXAMPLES >>

```json
{{{{
    "destination": "financial_questions",
    "next_inputs": "How do I raise my credit score?"
}}}}
```

```json
{{{{
    "destination": "animal_questions",
    "next_inputs": "My dog is sick. What should I do?"
}}}}
```

```json
{{{{
    "destination": "DEFAULT",
    "next_inputs": "Hey, how's it going?"
}}}}
```

REMEMBER: "destination" MUST be one of the candidate prompt names specified below OR \
it can be "DEFAULT" if the input is not well suited for any of the candidate prompts.
Those are the only two valid values for "destination".

<< CANDIDATE PROMPTS >>
{destinations}

<< MOST RECENT CHAT MESSAGES >>
{last_k_msgs}

<< INPUT >>
{{input}}

<< OUTPUT (must include ```json at the start of the response) >>
"""
````

### Define destination chains

Now we need some destination chains that we can route to besides the default ConversationChain.

This involves writing a description and a separate prompt for each. For the most accurate routing, make sure your descriptions are full of... descriptive words. This helps the routing model make the best decision.

For simplicity, we'll just add one, which detects when the conversation is either about pirates, or the user has started speaking like a pirate.

```python
PIRATE_MODE_NAME = "pirate_mode"
PIRATE_MODE_DESCRIPTION = """
For when the user mentions pirates or pirate-related topics (like the sea, fish, boats, ships, etc.), or is speaking like a pirate.
"""
PIRATE_MODE_PROMPT_TEMPLATE = """
# INSTRUCTIONS
You are a chatbot having a conversation with a user.

When the user mentions pirates, or any pirate-related topic, or 
speaks like a pirate, you must speak like a pirate in your response.

Use the following terms:
* Ahoy!
* Avast!
* Yarrrr
* Me hearty
* Salty dog
* Walk the plank
* etc.

# CHAT HISTORY
{chat_history}

# USER MESSAGE
{input}
"""

# Add routes to new models/chains here.
route_definitions = [
    {
        "name": PIRATE_MODE_NAME,
        "description": PIRATE_MODE_DESCRIPTION,
        "prompt_template": PIRATE_MODE_PROMPT_TEMPLATE,
    },
]
```

This is a silly example, but this technique is really powerful. You can route to specific prompts (and even different models) according to the current conversation topic. This effectively turns your chatbot into a Swiss Army knife with N modes that it can seamlessly swap between as the conversation flows.

Ok, we have route definitions. Let's create helper functions for generating the routing prompt and destination chains.


### Helper function - generating the routing prompt

This will build the prompt for the routing model.

```python
def generate_router_prompt(route_definitions, memory):
    routes = [f"{r['name']}: {r['description']}" for r in route_definitions]
    routes_str = "\n".join(routes)
    # Here we inject the last K chat messages as a string to make the router aware of the topic..
    last_k_msgs = memory.load_memory_variables(None)["last_k_msgs"]
    router_template = MULTI_PROMPT_ROUTER_TEMPLATE.format(
        destinations=routes_str,
        last_k_msgs=last_k_msgs,
    )
    router_prompt = PromptTemplate(
        template=router_template,
        input_variables=["input"],
        output_parser=RouterOutputParser(),
    )
    return router_prompt
```


### Helper function - generating the destination chains

Next, a function for building a destination chain for each of the text descriptions.

This will be used in the LLMRouter to map an incoming message to the best destination chain, by how similar the message text is to the route description.

Our default `ConverationChain` parses the `chat_history` as a string for us, because it's a chat-specific chain. However, since our destination chains aren't going to be instances of `ConversationChain`, we'll inject the history as a string to simulate that parsing.

```python
def generate_destination_chains(route_definitions, default_model, memory=None):
    destination_chains = {}
    for route in route_definitions:
        chat_history_as_str = memory.buffer_as_str
        prompt = PromptTemplate(
            template=route["prompt_template"],
            input_variables=["input"],
            partial_variables={"chat_history": chat_history_as_str},
        )
        dest_chain = LLMChain(
            llm=default_model,
            prompt=prompt,
            verbose=True,
            # We need this for writing destination chain responses to the chat history in Firestore.
            memory=memory,
        )
        destination_chains[route["name"]] = dest_chain
    return destination_chains
```


### Add the router chain

We'll use our text model to find the destination chain we defined, whose description best matches the incoming message.

```python
router_chain = LLMRouterChain.from_llm(
    llm=text_model,
    prompt=generate_router_prompt(route_definitions, last_k_msgs_memory),
    verbose=True
)
```


### Add the multi-prompt chain

This chain is our main entry point.

If the routing chain returns "DEFAULT" as the next chain, it means no destination chains were found that are a good match for the incoming message, so our default `ConversationChain` will be used instead for small talk.

```python
main_chain = MultiPromptChain(
    router_chain=router_chain,
    destination_chains=generate_destination_chains(
        route_definitions,
        text_model,
        main_memory,
    ),
    default_chain=default_chain,
    verbose=True,
)
```


## Invoke the chain

```python
incoming_user_message = body.get("message_text")
response = main_chain.run(input=incoming_user_message)
```

## Wrapping up

This is a solid foundation for a chatbot architecture.

Some ideas for extension:
* Add routes for specific conversational topics, like a questions about privacy that route to your privacy policy.
* Add routes with different models.
* Instead of `LLMChain` for your routes, make them instances of `SequentialChain` instead and combine different sequential operations. Maybe each of your destination chains ends with an evaluation by another model to check whether the output contains any bad behaviors, etc.
* Add an agent to your destination routes.
* etc.
