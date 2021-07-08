---
title: 8 Wastes of Six Sigma, Applied to Software Engineering
date: 2021-07-08T11:19:22-08:00
layout: single
permalink: /8-wastes-of-six-sigma/
category: 'programming'
tags: ['best practices']
---

Six Sigma is basically the study of efficiency in manufacturing.

They've codified a list of efficiency antipatterns called the [8 wastes](https://goleansixsigma.com/8-wastes/).

These antipatterns are transferrable to the domain of software engineering and are useful to know in your own work.

The 8 wastes are:

* **Transportation/travel/motion waste**: iterating over an entire collection rather than using a sliding window/two pointers, for example.
* **Defects**: introducing bugs. Unexpected and unintended behavior.
* **Overproduction**: building something you don't actually need yet, and may not actually ever need. Also, using a data structure that takes more memory when a smaller one would suffice.
* **Waiting**: using blocking code when it's not necessary and asynchronous code is more appropriate.
* **Non-utilized talent**: not reusing existing features/libraries.
* **Inventory**: a backup of work/information that is sitting idle rather than being processed. Bottlenecks. Backpressure. Tech debt accumulation.
* **Extra processing**: building to more restrictive constraints than actually called for. Not starting with MVP and extending on subsequent iterations.

I've combined travel/transportation and motion here; within software engineering these are the same, unless you want to consider the waste of commuting to an office instead of working remotely.