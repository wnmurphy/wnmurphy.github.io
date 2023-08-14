---
title: Achieving maximal chatbot versatility with a fan-out chain pattern in LangChain
date: 2023-08-11T08:12:22-08:00
layout: single
permalink: /achieving-maximal-chatbot-versatility-w-fan-out-chain-pattern-in-langchain/
category: 'programming'
tags: ['langchain', 'chatbot', 'llm']
---

Using a single prompt for a chatbot quickly runs into limitations.

When you try to make it do more than one thing well, it doesn't.

When desiging the architecture for your chatbot, a "fan-out" chain pattern gives you the most versatility.

Use a `MultiPromptRouter` to dynamically route the incoming user message to the best chain. The `MultiPromptRouter` uses a model to match the incoming message to the best matching destination chain, according to the chain's description and the text of the incoming message.

This gives your chatbot the ability to use different conversational "modes" by routing to a destination chain (prompt + model) specifically for handling that mode.

These destination chains can even be agents, which adds another layer of versatility.

Diagram of the general idea:

```
user input > 
  MultiPromptRouter > 
    LLMRouter >
      * default: ConversationChain 
      * request_for_information: LLMChain + informational response prompt
      * scheduling_questions: scheduling_agent > GET request for open time slots
      * journal_entry: journal_entry_agent > POST request to write a new journal entry
      * record_weight: weight_logging_agent > POST request to log a new weight
      * privacy_questions: LLMChain + privacy prompt
      etc.
```

The net effect is that your chatbot becomes a Swiss Army knife, and you can easily rack in additional "modes" to extend it's skill set.

In my opinion, this should be the standard pattern for any chatbot that needs to do more than one thing, or respond to more than one topic.
