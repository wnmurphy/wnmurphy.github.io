---
title: Insights about software development I wish had clicked earlier

date: 2023-08-20T21:09:22-08:00
layout: single
permalink: /insights-about-software-development-i-wish-had-clicked-earlier/
category: 'programming'
tags: ['principles', 'philosophy']
--- 

A collection of insights I wish I'd had earlier in my careeer, that might be useful to others.

**Software is the art of representation.** Representations should be intuitive and natural. Code doesn't have to feel like slapping words together in a foreign language. Don't overthink it. Rather than "how do I munge code together to make it work?" ask "what real-world thing or process are we representing?" Then represent each thing or object in that process as simply as possible. If you need to represent a verb, write a simple function that does that one thing. If you need to represent a noun (object or entity, a "thing"), write a simple model/class.

**The programming language is a proxy for manipulating values in a memory table.** Allocating space in the table for a new value, assigning the value, updating the value, moving to a new location in the table. Most of this is abstracted away and done for you in modern programming languages. But when you write code, this is what you're actually doing under the hood. 

**You don't need to be good at math.** This need almost never comes up, and when it does, middle-school math is enough. You'll need to be able to perform basic estimation (multiplication) and recognize why nested iteration is bad (exponents), but that's about it.

**You do need to be good at or enjoy structural thinking.** Structural thinking is the ability to hold abstractions in your mind and step through them sequentially. Math and writing software both use structural thinking. Philosophy uses structural thinking.

**Don't blindly implement a feature someone told you was needed.** Always ask yourself, "What are we actually trying to do here? What is the big picture?" Then ask, "Forget how it actually is. If I could rewrite this from scratch, how should it be?" Then when you implement the feature, accomplish the former in a way that gets you closer to the latter.

**Simplicity scales. Complexity fails.** Complexity rots your codebase. Make everything as simple as possible. Simple is easy to understand, and easy to change later. Complex is difficult to understand and difficult to change later.

**Code grows outward.** Code evolves, iteratively, from a skeleton. Never build something because you assume you'll need it. You won't need it, or you'll need something different instead.

**The specific is your excuse to accomplish the general.** Everything you write should be driven by a specific, well-defined use case. When you accomplish your specific use case, do it in a general/reusable way, so that later you have a component you can either reuse or extend. The exact opposite of this is designing an entire system in advance, trying to anticipate every use case. The assumptions you baked in will certainly be wrong, and you will have implemented a ton of complexity that is a pain to change later. 

**Aim for the Minimum Viable Product (MVP).** When you implement a feature, write the absolute minimum code that achieves the goal, and do it in a way that isn't tightly coupled to (unnecessarily dependent on) anything. This makes it easy to change later.

**Success is the result of obscene iteration count in a feedback loop.** Success is a function of (1) some kind of feedback loop, combined with (2) an ungodly number of iterations. Paul McCartney and John Lennon are legendary songwriters, and no one ever heard the hundred crappy songs they wrote when they were 15. Deliver something of value immediately. Collect feedback. Iterate on it.

**Read code.**. Spend more time reading code than writing it. Reading code is more cognitively difficult, but as with writing, good writers are good readers.

**Learn by doing.** One of the best ways to get familiar in a new language is to solve toy problems on Code Wars or something, but then spend more time reviewing other people's solutions. You'll learn conventions of the language, as well as several different ways to accomplish the same thing.