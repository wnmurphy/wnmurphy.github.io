---
title: How to speak properly to an LLM

date: 2023-08-14T21:09:22-08:00
layout: single
permalink: /how-to-speak-properly-to-an-llm/
category: 'programming'
tags: ['llm']
---

An LLM is literally a text predicting machine. That's all it does. 

Because an LLM is a text predicting machine, the name of the game when writing a prompt is "influencing probabilities."


## The prompt influences probability

A good prompt has a high probability of resulting in the output you want.

A bad prompt has a low probability of resulting in the output you want.

The output is generated word by word according to the probability of each possible word that might follow in the current context.

If your input is the phrase "The quick brown fox jumped..." it is almost certain that the output will be the phrase "over the lazy dog."

This is because this is such a common sentence that it would be overrepresented in the training corpus for the model.

The least effective way to achieve that output phrase, would be to make your prompt something like "The fast taupe bobcat pounced..."

This is because the probability of the words you want as output following those input words is extremely low.

The art of prompt writing is all about influencing probabilities through word choice.

For this reason, you get the highest quality output when your input consists of more obvious expressions. 


## Speak plainly

In other words, speak as plainly as possible. Rather than a mindset of "what magical combination of words do I need to use to trick the model into doing what I want?" you should think instead in terms of stating literally what you're trying to achieve.

A good litmus test would be to look at your prompt and ask yourself, "if I received this prompt as a writing assignment for a job interview, would it be crystal clear to me what to do?""

Write your prompts like you are writing an interview assignment for job candidates.


## Start with the frame

When you write a prompt, imagine you are giving an acting scene to someone who is extremely good at improv, but who takes everything completely literally.

Play a little make-believe with it in your opening line, like `You are a helpful chatbot who specializes in summarizing a user's personal data in JSON format.`


## Give an explicit instruction

Simply state what you want the model to do, like `Summarize the data under ADDITIONAL INFORMATION in valid JSON format, like the EXAMPLES`.


## Use markdown

Markdown is fantastic in general, but it's really good for prompts because it allows you to create information mapping and separate parts of the prompt by category. You can also use headings to nest information like few-shot examples, and you can use code blocks to encapsulate responses that you want as JSON or some other language.

```
# INSTRUCTIONS
You are a helpful chatbot who specializes in summarizing a user's personal data.
Summarize the data under ADDITIONAL INFORMATION in valid JSON format, like the EXAMPLES

# ADDITIONAL INFORMATION
{context_data}

# EXAMPLES

{ "summary": "The user has 3 pairs of gym socks, 2 rolls of duct tape, and 1 chunk of beef jerky in their gym bag today."}

{ "summary": "The user has 1 copy of Proust's 'In Search of Lost Time' in their gym bag today."}

{ "summary": "The user has nothing in their gym bag today."}

# SUMMARY
```


## Frame it positively

Include instructions that focus on the output you _do_ want, rather than avoiding the output you _don't_ want. You'll get better results.

You can include information about penalties for unwanted behavior, but these aren't as effective as describing the desired behavior.

If you're trying to eliminate bad behavior, try using LangChain's `ConstitutionalPrinciple`, which allows you to define "values" for your chatbot, use a model to evaluate whether the output violates any, and then use revision instructions to rewrite the output according to the principle.


## Eliminate ambiguity 

Once your prompt is re-written in plain English, do another pass to identify any words or phrases that could be misinterpreted.

Imagine that you are a lawyer drafting a legal document, and your job is to make sure there is no wiggle room for misinterpretation.

* Do any words have more than one meaning?
* Is the meaning of any word unclear in the context?
* Are any of the words jargon, terms that the average person wouldn't immediately understand?
* Can you eliminate any words or phrases completely, without losing any meaning?
* etc.


## Less is more

When you write a prompt, don't try to solve a problem in advance that you don't actually have yet.

The more instructions in a prompt, the harder it is for a model to pay attention to what's important. If everything is important, nothing is. Therefore, you should add sentences sparingly.

A good example is instructions about tone. You'll probably find that the model's tone is just fine as it is. You probably don't need to add instructions about being friendly, etc.

Don't add instructions to a prompt, unless it's clear that it solves a problem you actually have.


## A few other things to remember


### LLMs are stateless

An LLM doesn't know anything about the world, except the text that you give it as input.

If you want it to be aware of any information, you need to pass that in as part of the prompt.


### LLMs are purely reactive

It's not capable of initiating actions. It's entirely reactive.

If you think an LLM is a good solution for a use case, ask yourself if that use case requires initiating any actions.
