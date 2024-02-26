---
title: Generative AI/chatbot best practices
date: 2024-02-26T08:12:22-08:00
layout: single
permalink: /generative-ai-chatbot-best-practices
category: 'programming'
tags: ['langchain', 'chatbot', 'llm', 'best practices']
---

Over the last year, each of us in the industry has been learning how to integrate generative AI together, mostly exploring and figuring things out as we've gone along.

This is a collection of lessons I've learned from rapid iteration and experimentation in developing a chatbot healthcare assistant.

## Use a router that selects the right prompt/tool based on the incoming message

Using a router makes your chatbot "multi-modal" in the sense of having N different modes that it can dynamically select from. This dramatically improves handling of topics by having a dedicated prompt for each topic.

## If you're using langchain, use agents/tools as routes rather than LLMChains

You're inevitably going to want your chatbot to be able to take programmatic actions, like calling an API for another of your services. The default multi-prompt router only supports using `LLMChain` destination routes, which limit you to a single model call with a single prompt.

You can probably adapt an agent/tool to an `LLMChain` interface so it can be used in the router, but I think it's simpler to adapt an `LLMChain` to an agent/tool interface. If I started over again, this is one thing I would explore doing differently.

## Make your default route a conversation model

I found that using a conversational model to drive your default route will smooth over most rough spots in the conversation.

When the router can't match to the correct prompt, then at least it will be handled gracefully in a way that feels natural to the user.

This approach was later recommended by seminar speakers at Google's 2023 Data Cloud & AI Summit, which validated the approach and confirmed that others had also discovered this as a best practice.

## Use short, targeted prompts and then compose them

Your prompt is effectively a function. As with functions, each prompt should be a [small, sharp tool](https://brandur.org/small-sharp-tools).

Output quality decreases as prompt length increases. We learned early on that you can't just cram everything into a prompt and expect the model to know what to do and do the right thing.

We initially tried aggregating a blob of user data, thinking we could inject that into a prompt and the model would just understand it. Not only did it not use the data effectively, but it paid less attention to the other instructions.

Use focused, shorter prompts and then compose model calls.

## Use consistent terminology within and across your prompts

LLMs are associative by nature; they effectively encode the relationships between tokens in a domain language.

If you use several different words that mean the same thing throughout your prompt, those associations will be weaker, and your results will not be as consistent as if you standardize to a single term and stick with it.

Double check your prompts to make sure that they all use the same term consistently to refer to the same thing.

## Expand the user's message into a user intent statement

This was a big one.

Use an LLM to expand the literal text of an user's incoming message into a summary of the user's intent. Then, use this intent statement instead of (or in addition to) the literal message for your routing and content matching (RAG), and inject it into your final prompt at the very end.

I found that this significantly improved:
* Router accuracy (selecting the right prompt to use).
* Content matching.
* The response from the final model call.
* Contextual topic awareness.
* Handling of low-effort, one-word messages.

Place a model call at the start of your chain/flow, with a prompt like:
```
Given the user's INCOMING MESSAGE and the context of the recent CHAT HISTORY, identify the user's current intent and provide a brief summary that captures the main goal of their query as accurately as possible given the available information.
The INCOMING MESSAGE may represent a new intent unrelated to prior topics in the CHAT HISTORY. If so, only mention the new intent.
Begin your responses with "The user".

# CHAT HISTORY
{most_recent_four_messages}

# INCOMING MESSAGE
{incoming_message}

# USER'S CURRENT INTENT
```

Incorporate into your router prompt:
```
Given the user's current intent and their incoming message, select the route best suited to fulfill that intent.
You will be given the names of the available route and a description of what each route is best suited for.
<< OUTPUT FORMAT >>
Return a markdown code snippet with a JSON object formatted to look like:
 json
 {{{{
     "destination": string \\ key of the route to use or "DEFAULT"
     "next_inputs": {{{{
         "input": string \\ the incoming message
         "current_intent": string \\ the user's current intent
     }}}}
 }}}}
```

The beauty of this as a best practice is that it also give you contextual topic awareness for free when you inject the most recent 4 messages. 

The summary of the user's intent is also a statement of the current topic, and placing intent identification with chat history at the front of your architecture makes everything downstream aware of whether the current message represents a new topic, or a continuation/expansion of the current topic.

When the model asks a clarifying question, or the user tacks on a confirmation or negation, the intent is then reinterpreted in the context of the chat history, so all of the user's recent messages on the same topic are considered together.

```
User: taco
[Intent: The user may be requesting information about tacos, or a taco recipe.]
Chatbot: Would you like information about tacos, or a taco recipe?
User: recipe
[Intent: The user has clarified that they would like a taco recipe.]
Chatbot: Sure! Here you go... $TACO_RECIPE
User: no lettuce
[Intent: The user has clarified that they would like a taco recipe modified to exclude lettuce.]
Chatbot: Of course, here's a recipe without lettuce... $TACO_RECIPE_MINUS_LETTUCE
```

## Increase temperature to improve compliance with instructions

Paradoxically, I've found that compliance with a list of instructions *increases* when temperature increases.

It's as if your model parameters can sometimes paint the model into a corner, where it doesn't have enough available options to generate output that follows your instructions.

If your model isn't behaving, try bumping up the temperature. Give it a wider lane, and then trust it to generally stay in the middle.

## Suppress outbound links with prompt instructions + a route that handles requests for them

One of our product requirements was that the chatbot not serve any outbound links. Not only are these prone to hallucination, but we also have controls on which sources are approved for medical/health information for our users.

Outside of constitutional AI (placing a final call to evaluate whether any constitutional principles have been violated, then revising the output), the next best way to handle this is to add a route to detect requests for links and resources, and handle these with something like `I'm not able to send links, but a web search should help you find what you need`, etc. Then, add a clause to prompts for your other routes like `...without sending outbound links or URLs`.

## Use the prompt to get multi-language  support/localization for free

We track our user's preferred language. Since LLMs are associative by nature, they are also (un)surprisingly good at translation.

I found that adding a line `Respond in the user's preferred language, or English if not set` effectively gets you localization for free.

Is it perfect? Maybe not, but I'll leave that to the translators to assess. It's pretty good considering that it costs nearly no additional work.

## Validate unsupported characters as soon as you get the user's message

If a user is trying script injection with prompt-y, Jinja-y chars like `{` or `}`, you're going to have a bad time. These will break prompts, and if you're injecting chat history that contains a message with these chars, then that user will no longer be able to use the chatbot (every response will load the naughty message and break). 

These can also occur if the user asks for an example of Hello World in Java, etc.

The approach I used was to validate messages immediately to catch these on the inbound side, as well as to add a route specifically for requests for code to catch these on the outbound side.

## Use few shot examples for longer prompts

If you have longer prompts where you need to inject a lot of data, including a section with few shot examples is really effective. These basically "prime the probabilistic pump," adding weight to the relationships they illustrate.

This can counter the effect where increasing prompt length decreases quality (adding more elements makes each element less important).

## Evaluate different models and use the best

We discovered that using PaLM's `textembedding-gecko@003` for our embeddings and vector store queries was significantly inferior to OpenAI's `text-embedding-3`, with an f-score (harmonic mean of the precision and recall) of 0.30 vs. 0.85. In fact, the latter was on par with PaLM's Gemini encoder.

This was surprising, because we're a Google Cloud Platform shop, but illustrates that in terms of performance for your architecture, it pays to evaluate and shop around.

## Display generated follow-up messages for the user to tap on

The average user doesn't understand yet how to interact with a chatbot. We found significantly increased engagement once we added a feature that just displays 3 potential responses the user can send with a single tap (see: Smart Replies in Android SMS messages).

This also has the benefit of being "self-onboarding," meaning that it's immediately intuitive to the user what these are for, and they also teach the user how to interact with the chatbot, i.e. what kinds of messages can be sent.

Implementing this is dead simple too. Since LLMs predict, you just need to pass the chat history into the model with the most recent user message appended, and the output is a candidate response.

## Use a single model call to create a pseudo-batch of predictions

Adding to the last concept, each call can add 1-2s to your total latency. Rather than ask for N predictions in a batch, it's more efficient to include instructions to `generate an array of N strings`, etc. and then parse the output to JSON to validate it.

For smart replies, I'm asking the model for 3 messages that the user could send in response to the chat history, with an emphasis on questions; these are more engaging and facilitate continuation of the conversation.

## Log every prediction

Log everything. You're going to want the compiled prompt, the model used, the prompt used, the model parameters used, the chat history, the user's message, the user's intent, the user ID, the model's prediction, any content matching during RAG, the RAG parameters, the RAG results and their distance, etc.

Just log everything.

## Evaluate everything

If you're not evaluating, you're navigating by feel alone. Because generative AI is probabilistic for any one prediction, and statistical across all predictions, you have no actual idea whether a change has made the end user experience better or worse unless you run evaluations and split test.

At a minimum, I recommend evaluations for router precision and recall (combined into f-score), RAG answer (summary) relevance, RAG content (matching chunks) relevance.

If you haven't created evals yet, you basically need to create a synthetic data set, label it with the expected attributes (routes), run your component (router, etc.) then evaluate actual results against the expected label.

## Make your user aware of the features

We found out from early rollout surveys that most of our users weren't even aware that we'd given them a chatbot assistant.

This might be obvious, but if the user doesn't know they have a chatbot, they won't use it.

## Understand who your user is and how they feel

Most of your users are not going to be techies who think gen AI is fascinating. Many of them find AI of any kind scary and weird, and probably have an aversion to it before they even come to your feature. Make sure you keep this in mind when choosing a name/persona.

For people to trust AI, it needs to be personable and welcoming.

## Understand how your user actually uses the feature

Most of your users are not going to send eloquent messages. They will treat your chatbot like an AT&T or Comcast voicemail menu, using one-word commands or phrases like "operator" or "billing."

To handle this effectively, use the intent expansion technique listed above.
