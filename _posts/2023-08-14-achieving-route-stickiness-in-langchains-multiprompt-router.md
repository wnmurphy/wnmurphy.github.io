---
title: Achieving route stickiness in LangChain's multi-prompt routing

date: 2023-08-14T16:12:22-08:00
layout: single
permalink: /achieving-route-stickiness-in-langchains-multiprompt-router/
category: 'programming'
tags: ['langchain', 'chatbot', llm']
---

LangChain's `MultiPromptRouter` is a powerful tool you can use to dynamically route an incoming message to chains with different models depending on the message's content. This is really powerful, because it enables you to easily hook up different "brains" for your chatbot.

For example: you can have PaLM `chat-bison` as your default model in a `ConversationChain`, and then if the user asks a medical question, it will route to MedPaLM with a "virtual doctor" prompt. Your chatbot is now a virtual doctor.

You can also use this technique to route to chains that have the same model, but different prompts. This enables you to have different "modes" of conversation depending on the topic, by defining handlers for those topics.

However, the `MultiPromptRouter` only considers the incoming message when choosing a destination chain. In a chatbot application, this can result in noticeable switches between routes/chains and create an unnatural chat experience.

To make transitions more seamless in conversation, you can add a `ConversationWindowBufferMemory` as a secondary memory just for the routing prompt, with a lookback of 2 messages.

By injecting this chat history with a short lookback into the prompt, and modifying the prompt slightly so that the routing model considers the current topic as well incoming message, you can create a router that will "stick" to a given route even if the literal text of the incoming message suggests a different route.

Imagine you're engaging in small talk with a `ConversationChain`, but then you start talking about feeling down lately, which activates the virtual therapist/"empathetic listening" route. You want to stay in that mode for a while as you talk about your feelings.

With the default router, as soon as you send a message that looks like small talk, you'll be back routed to the conversation model:

```
Human: Thanks, that's good advice.
AI: I'm glad! Happy to help.
```

However, with a 2-message memory added to the routing itself, the router model now has a better understanding of what the current topic actually is.

You'll stay in the virtual therapist mode and get something more like:

```
Human: Thanks, that's good advice.
AI: I'm glad. It can be difficult to deal with these things, but it's helpful to talk about. I'm here if you need me.
```

This gives the routing feature a wider contextual window which makes it aware of the broader topic of conversation, creating a better sense of continuity and more seamless transitions between destination chains or "modes" of your chatbot.


## How to do this

First, create your own copy of the routing prompt, located in `langchain/chains/router/multi_prompt_prompt.py`:

Original:

````python
MULTI_PROMPT_ROUTER_TEMPLATE = """\
Given a raw text input to a language model select the model prompt best suited for \
the input. You will be given the names of the available prompts and a description of \
what the prompt is best suited for. You may also revise the original input if you \
think that revising it will ultimately lead to a better response from the language \
model.

<< FORMATTING >>
Return a markdown code snippet with a JSON object formatted to look like:
```json
{{{{
    "destination": string \\ name of the prompt to use or "DEFAULT"
    "next_inputs": string \\ a potentially modified version of the original input
}}}}
```

REMEMBER: "destination" MUST be one of the candidate prompt names specified below OR \
it can be "DEFAULT" if the input is not well suited for any of the candidate prompts.
REMEMBER: "next_inputs" can just be the original input if you don't think any \
modifications are needed.

<< CANDIDATE PROMPTS >>
{destinations}

<< INPUT >>
{{input}}

<< OUTPUT (must include ```json at the start of the response) >>
"""
````

I dislike that it allows the routing model to modify the incoming text, because this creates a false chat history, so I remove that language. I also inject my `most_recent_k_msgs` in a new section and add instruction to consider them with weight.

````python
MULTI_PROMPT_ROUTER_TEMPLATE = """
Given a raw text input to a language model select the model prompt best suited for 
the input. You will be given the names of the available prompts and a description of 
what the prompt is best suited for.

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

Then I use this prompt in my router. I'm passing

```python
def generate_router_prompt(route_definitions, router_memory):
	routes = [f"{r['name']}: {r['description']}" for r in route_definitions]
	routes_str = "\n".join(routes)
	last_k_msgs = router_memory.load_memory_variables(None)["last_k_msgs"]
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

I create a secondary memory, just for the router:

```python
router_memory = ConversationBufferWindowMemory(
	memory_key="last_k_msgs",
	chat_memory=chat_history_store,
	return_messages=False, # Parse as text; we'll inject directly
	k=2, # Grab the last 2 messages only
)
```

Then I pass this router memory into the router:

```python
router_chain = LLMRouterChain.from_llm(
	llm=text_model,
	prompt=generate_router_prompt(route_definitions, router_memory),
	verbose=settings.CHAIN_VERBOSITY,
)
```

That's it! 

So now, when routing, the routing model will be aware of the current topic, and will continue to route to the same destination chain until the topic actually changes.

This is a simple, but very effective way to make transitions between converational modes much more natural and seamless.
