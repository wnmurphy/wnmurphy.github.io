---
title: LLMs can't do humor
date: 2026-04-08T10:12:22-7:00
layout: single
permalink: /llms-cant-do-humor/
category: 'programming'
tags: ['llm', 'humor']
---

One thing that fascinates me about llms is that they seem to be incapable of producing anything funny, or at least intentionally funny. 

I suspect that this is due to the nature of what an LLM is: a probabilistic word-chunk predicting machine, where the probability of each subsequent chunk is considered in the context of the preceding text.

Another way to think of an LLM is as a function that represents every valid path through a high dimensional vector space, where a "valid path" is a contextually meaningful sentence encoded by the model's weights.

If you think about what humor is, it's essentially a violation of expectations. The setup of a joke establishes a probabilistic groove, creating the expectation that the punchline should continue in that direction.

A good (funny) punchline represents an orthogonal departure from that groove. It breaks that probabilistic expectation that the content will continue in that direction. Instead it takes a hard right turn and drops you off somewhere unexpected.

The human mind responds to this tension between what was expected and what actually happened, by laughing. Laughing is essentially a tension release response.

So in order for something to be funny, it needs to make a sudden orthogonal detour from the valid path being traced probabilistically through the vector space when you call the LLM for inference. It needs to violate those probabilities.

An LLM cannot do that, however, because it would break the very function of an LLM; humor requires a sudden violation of the probabilities an LLM is designed to follow.
