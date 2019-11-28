---
title: SOLID Principles Cheat Sheet
date: 2019-11-28T11:19:22-08:00
layout: single
category: 'programming'
tags: ['best practices']
---

[SOLID](https://en.wikipedia.org/wiki/SOLID) is an acronym representing 5 principles of object-oriented programming that inform the construction of well-designed, maintainable components. These principles apply to functions, classes, microservices, etc. 

If you've had the experience of recognizing that code is well-written without being able to put your finger on exactly why, it was probably the application of these principles. 

The original source of these principles is Robert C. Martin's paper, _[Design Principles and Design Patterns (2000)](/assets/pdf/Robert_C._Martin_-_2000_-_Principles_and_Patterns.pdf)_. 

Martin also wrote a book called _[Clean Code: A Handbook of Agile Software Craftsmanship](https://amzn.to/2rDvxz7)_ that every developer should read; it contains these principles and many more best practices that every developer should have deeply engrained.

Ryan McDermott has a great [repo](https://github.com/ryanmcdermott/clean-code-javascript#solid) illustrating _Clean Code_ principles (including examples of SOLID) applied to JavaScript.

This is my own cheat sheet, so if you're already familiar with these principles you might notice that I've paraphrased them here, using different wording.

## Single Responsibility Principle (SRP) 

> Each component should do one thing.

Write components that each do exactly one thing. Any related tasks should be factored out into their own components, and composed together.

If your component's name has the word "and" in it, it's a sign you need to break that component into separate parts.

## Open-Closed Principle (OCP)

> A component should be extensible, not modifiable.

Write components that are extensible, so that no one ever has to modify them.

Strategy 1: Write the "what," and leave the "how" up to the caller.

Strategy 2: Make everything a subclass. The subclasses extend the superclass, which cannot be modified.

I've heard it expressed that "modification is the 'most insulting' thing you can do to a component." This is why we have regression testing, why good engineer prefer the lightest touch, and why you should ideally have complete test coverage in place before you perform major reconstructive surgery (a.k.a. 'refactoring').

## Liskov Substitution Principle (LSP)

> You should be able to swap in a subclass without problems.
  
A subclass must entail superclass behavior. The extension of a component should do everything the original component does.

Subclass output (return values and errors) may be a subset of the superclass, but their interface (methods and their arguments) must be identical.

If you hire a master contractor, but he sends his apprentice instead, the apprentice may be specialized (hanging drywall, maybe), but he needs to correctly handle all of the jobs the master can do to the same standard.

## Interface Segregation Principle (ISP)

> Create specialized, targeted interfaces.

No component should be forced to depend on methods it does not use.

An (adequate) analogy: if you're designing a motherboard, don't combine the printer connection with the monitor connection. If I'm designing a monitor, I don't want to have to care about printers.

## Dependency Inversion Principle (DIP):

> Inject everything a function needs to do it's job, and no more.

Pass dependencies into components, rather than load them globally.

A component should not know any more about the world than it needs in order to fulfill it's single purpose.

See any native function that takes a callback like `Array.prototype.map`. It doesn't need to know anything more than: 
1. the fact that it needs to iterate over an input array,
2. the fact that it needs to produce a new output array,
3. the specific way you want it to accomplish (2) given (1).

`.map`'s only dependency, then, is (3): how exactly it should map the values. So we pass it in as a callback.

This principle is analogous to a "need-to-know" security clearance for classified information, or the "principle of least privilege."

If a component just needs a value, not the entire object containing the value, then just hand it the value. 

If you're just hammering a nail in, you don't need to retreive the entire toolbox from the garage. Just bring the hammer.