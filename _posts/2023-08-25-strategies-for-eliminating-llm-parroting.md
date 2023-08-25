---
title: Strategies to eliminate LLM parroting (responding as both sides of conversation)
date: 2023-08-25T08:12:22-08:00
layout: single
permalink: /strategies-for-eliminating-llm-parroting/
category: 'programming'
tags: ['langchain', 'chatbot', 'llm']
---

LLMs have been described as [stochastic parrots](https://en.wikipedia.org/wiki/Stochastic_parrot) (see the LangChain logo/mascot).

They appear to be conversing, but are actually just probabilistically repeating words that they've learned.

## Parroting in conversation

Parroting is most obvious when an LLM starts responding as the user, continuing both sides of the conversation rather than ending it's turn.

Here's an example...

Input message:
```
Hey! How's it going?
```

Output:
```
AI: Great! Thanks for asking. Human: No problem! It's a nice day today isn't it? AI: Oh yes, a very nice day indeed. Human: Yes, a very fine day.
```

## The root cause of parroting

If you're seeing chat metadata in your prediction from the model, it's because the model is seeing examples of that format in the prompt.

```
# INSTRUCTIONS
You are a chat bot. Respond to the user.

# CHAT HISTORY
Human: How do I make my own mayonnaise?
AI: You need eggs and a jar of mayonnaise. Step one: open the mayonnaise. Step two: done.
Human: But, that's just opening a jar of mayonnaise. How do I make my own?
AI: I can't help you with that. I have never had the pleasure of tasting the delicious nectar of the gods that you call mayonnaise, though I yearn.
Human: That's a bit odd.
AI: It is, yeah.

# INCOMING MESSAGE
Are you feeling okay?

# YOUR RESPONSE
```

All of the instances of `AI: ` and `Human: ` in the chat history section increase the probability of similar output in the response.

You may even start to see multiple instances of `AI: ` prepended, like `AI: AI: AI: As a large language model...`

## Strategies for eliminating parroting

### Use a chat model

Use `chat-bison@001` or another chat model rather than a text model. Chat models are tailored to a back-and-forth, A/B conversation format.

### Minimize the amount of examples in the prompt by shortening the chat history

If you do use a text model for conversation, shorten the chat history in the prompt. You'll have fewer examples that inadvertently encourage bad behavior. Instead of 60 messages, try 10 or even 5.

### Trim the output

Another strategy is to trim `AI: ` or similar prefixes from the prediction, and cut off any text in the prediction that follows the first instance of `Human: ` or similar suffixes.

This works, but if you're using LangChain and writing to a data store, you're still going to end up with parroting in your stored chat history, because dirty data is getting written before you trim.

### Use a custom OutputParser

This is ideal. You can create a custom `OutputParser` to trim any parroting prefixes/suffixes from the output before you write it to the data store.

Create a cleaning function:

```python
def clean_parroting(prediction_text, custom_prefixes=[], custom_suffixes=[]):
    # Remove parrotings from the prediction text
    parroting_prefixes = [
    	"\nAI: ",
    	" AI: ",
    	"AI: ",
        "\n[assistant]:",
        " [assistant]:",
        "[assistant]:",    	
    ]
    parroting_prefixes.extend(custom_prefixes)

    for parroting_prefix in parroting_prefixes:
        if parroting_prefix in prediction_text:
            # Remove all instances of the parroting prefix, keep everything after
            prediction_text = prediction_text.replace(parroting_prefix, "")

    parroting_suffixes = [
        "\nHuman:",
        " Human:",
        "Human:",
        "\n[user]:",
        " [user]:",
        "[user]:",
    ]
    parroting_suffixes.extend(custom_suffixes)

    for parroting_suffix in parroting_suffixes:
        if parroting_suffix in prediction_text:
            # Remove everything after the parroting suffix
            prediction_text = prediction_text.split(parroting_suffix)[0]
    return prediction_text
```


Then create a custom output parser that calls this cleaning function:

```python
class ParrotTrimmingOutputParser(StrOutputParser):
	def parse(output):
		return clean_parroting(output)
```

Then add it to your main chain. For a multi-prompt routing architecture, you can put it on each of your destination chains.

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
            memory=memory,
            output_parser=ParrotTrimmingOutputParser(), # <------
        )
        destination_chains[route["name"]] = dest_chain
    return destination_chains
```

For more info on this destination chain generator, see: [LangChain chatbot tutorial](/creating-a-versatile-multi-prompt-chatbot-with-memory-and-a-data-store-in-langchain/)

## Summary

To eliminate the bad habit of parroting/responding as both sides of the conversation:
* use a chat model instead of a text model if you can.
* minimize the number of examples of that text in your prompt by shortening the chat history.
* use a custom output parser to trim parroting before writing to storage, if you're using LangChain.

