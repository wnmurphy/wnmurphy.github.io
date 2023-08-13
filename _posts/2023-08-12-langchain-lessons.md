---
title: LangChain lessons and observations
date: 2023-08-12T16:12:22-08:00
layout: single
permalink: /langchain-lessons/
category: 'programming'
tags: ['langchain', 'llm']
---

This is mostly a scratch for me to refer to, as I learn, but figured it might also be helpful someone else.

## Chains in a nutshell

A chain is an abstraction that encapsulates a prompt template + variable injection + a model + any memory-related operations.

A model chain (LLMChain) takes:
  * an input key aliasing the initial input to the chain (input key `input` by default).
  * (optionally) a memory class (memory key `history`).
  * a prompt template.
  * a list of input variables expected by the prompt as `input_variables`.
  * a model to use for predictions.
  * (optionally) an output parser.
  * an output variable which is text by default, response in a ConversationChain, or a custom variable if set.
  * (optionally) a list of callbacks to execute on different chain events.

When run, the chain:
 * takes the initial input aliased as `input`, or `**kwargs` if multiple inputs.
 * (optionally) loads the memory, if set on the chain, aliased as the memory key `history`.
 * injects the input into the prompt template.
 * sends the prompt template in to the model.
 * (optionally) parses the model’s output.
 * (optionally) writes the output to memory, if set on the chain.
 * which optionally writes the output to a data store, if set on the memory.
 * returns the model’s output as the output variable.

 ## Miscellaneous lessons

It can be difficult to understand what's happening under the hood, why you're getting errors related to input/output keys, etc. This is especially true once you start connecting multiple chains and other components. To get visibility into what each chain is getting for input and output, add a breakpoint in the `Chain._call` and `Chain.prep_inputs` methods.

If you have chat memory with a data store hooked up, saving to the data store occurs in `BaseChatMemory.save_context`.

If you need LangChain to do something it doesn't do yet, open a PR and contribute. It's open source, and they release a new minor or patch version every day or two. Make life easier for the reviewers by including an explicit branch name, and a good description of exactly what your PR does, including use cases. This will expedite your contribution.

Prefer Jinja2 templates over f-string templates, because f-string doesn't support iteration over a list or other operations that Jinja2 does for you. For example, with f-strings you can't pass a list of values into a prompt and have it expanded into a bullet list.

Because this stuff is so new, there isn't a lot of documentation. If you're stuck on a bug, try searching all of Github for the class name that you're using, or a unique snippet of the function signature. This can help surface ideas about how others are using the same class.


## Common errors and what they mean

### Error:

`Invalid prompt schema; check for mismatched or missing input parameters. 'history' (type=value_error)`

### Cause

Your `PromptTemplate` is receiving a `history` variable in `input_variables` which isn't in the template. Add the variable to your prompt, or check variable names to make sure they're the same.


### Error:

`Missing some input keys: {'some_input_key'}`

### Cause

Your PromptTemplate specifies an input key `some_input_key` that isn't being passed in when the chain is invoked with `.run`.

