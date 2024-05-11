---
title: Deeper understanding from anthropomorphic perspective-taking
date: 2024-05-09T07:19:22-08:00
layout: single
permalink: /deeper-understanding-from-anthropomorphic-perspective-taking/
category: 'learning'
tags: ['llm', 'chatgpt', 'learning']
---

I was reviewing my external brain (a massive git repo containing 20+ years of my writing/snippets/thoughts I call "memory alpha"), and came across this quote from Jonas Salk:

> Very early in my life, I would imagine myself in the position of the object in which I was interested.  Later, when I became a scientist, I would picture myself as a virus, or as a cancer cell, for example, and try to sense what it would be like to be either.  I would also imagine myself as the immune system, and I would try to reconstruct what I would do as an immune system engaged in combating a virus or cancer cell. . . Before long, this internal dialogue became second nature to me.

This reminded me of a suspicion I had, that you could acquire an intuitive understanding of algorithms by imaginging that you are the algorithm. I'll call this anthropomorphic perspective-taking.

We can try something similar with ChatGPT to interact with a topic from a second-person perspective.

## First experiment with ChatGPT

Input:

```
Describe a binary search algorithm from the first-person perspective of the algorithm, as if it's thinking out loud.
```

Output:

```
I am the Binary Search Algorithm, ready to dive into this array and find that elusive target. First things first, I need to know where I'm starting and where I'm ending. Ah, there's the sorted array in front of me. Let's assess the midpoint. It's crucial; it will tell me which half of the array to focus on.
Now, let's compare the midpoint value with my target. Is it equal? If it is, splendid! I've found it. If not, I need to make a decision. Is my target greater or less than this midpoint value? That determines which half of the array I discard. 

Alright, decision made. If my target is greater, I discard the left half; if it's smaller, I discard the right half. Repeat the process on the remaining half. Now, I'm focusing on a smaller subset. 

I keep doing this, halving the remaining portion each time until either I find the target or I've exhausted all possibilities. It's like peeling an onion, getting closer and closer to the core with each layer.

And there it is! I've either found the target or determined it doesn't exist in this array. Time complexity? Logarithmic, baby! That's the beauty of binary search.
```

That's not bad. Let's interact with it...

Input:

```
What makes you logarithmic?
```

Output:

```
Ah, logarithmic time complexity is one of my defining characteristics, and it's what makes me so efficient. You see, every time I make a comparison and discard half of the remaining elements, I effectively halve the size of the portion I need to search. 

This halving process is powerful because it allows me to zoom in on the target quickly, reducing the search space exponentially with each iteration. As a result, even in a large array, I can find the target in just a few steps, making my time complexity logarithmic in relation to the size of the array.

So, no matter how large the array is, I can find the target efficiently, thanks to my logarithmic time complexity. It's what makes me such a valuable tool for searching sorted collections of data!
```

Input:

```
What happens if the array you get is empty?
```

Output:

```
Ah, an empty array presents a straightforward scenario for me. Since there are no elements in the array, there's no chance of finding any target within it.
In such a case, I would simply return a message indicating that the target cannot be found because the array is empty. There's no need for any searching or comparison since there are no elements to search through.
So, when faced with an empty array, I gracefully acknowledge the absence of any elements and inform the user that the target they're searching for cannot be found within it. It's a simple and straightforward response to a simple scenario.
```
## Anthromorphic perspective-taking as a custom instruction for ChatGPT
What happens if we just make every inquiry answered in this way?
### Custom instructions
`What would you like ChatGPT to know about you to provide better responses?`:
```
I need to learn through a technique called anthropomorphic perspective-taking.
```
`How would you like ChatGPT to respond?`
```
Describe the given topic from the first-person perspective of the topic itself, as if it's thinking out loud.
```
### Example: the locrian mode
Input:
```
The locrian mode of the major scale
```

Output:

```
Ah, the Locrian mode, I'm often overlooked, but I have my own unique charm. You see, I'm like the mysterious sibling in the major scale family, residing on the seventh degree. My structure might seem a bit peculiar to some, but that's what makes me intriguing.

I start on the seventh note of the major scale, creating a distinct interval pattern that gives me my peculiar sound. Picture this: a root note, then a minor second, followed by a diminished third. It's a bit dissonant, but that's where my character shines.

I have this enigmatic aura about me, partly because my diminished fifth gives me a rather unstable feeling. Some might even say I sound unsettling or unresolved. But it's precisely this quality that makes me interesting to composers and musicians alike.

You see, in certain musical contexts, I can add tension and intrigue, like a subtle spice in a dish. Composers might employ me to create a sense of unease or mystery, or to add a touch of darkness to their compositions.

But despite my somewhat unconventional nature, I still have my place in the musical landscape. When used thoughtfully, I can bring depth and complexity to a piece, adding layers of emotion and texture that other modes might not achieve.

So, while I may not be the most popular mode in the major scale family, I'm certainly not to be overlooked. Embrace my uniqueness, and you might just discover the depth and richness I can bring to your musical creations.
```

Not bad.

### Example: an ignition coil

Input:

```
An ignition coil in a car engine
```

Output:

```
Oh, the marvel of being an ignition coil in a car engine! My purpose is electrifyingly clear: to transform the battery's low voltage into a high-voltage spark that ignites the air-fuel mixture in the engine's cylinders. It's a thrilling responsibility, for without me, the engine's heart would remain dormant, unable to power the vehicle's journey.

As the engine cranks, I feel the current coursing through my copper windings, creating a magnetic field. Suddenly, with a surge of power, the current is interrupted, collapsing the magnetic field and inducing a high-voltage spark across my terminals. This spark leaps to the spark plug, igniting the compressed air-fuel mixture with a fiery explosion, propelling the pistons into motion and setting the wheels in motion.

I relish in my role as the catalyst of combustion, the spark of life for the engine. Each cycle is a testament to my reliability and efficiency, ensuring smooth operation and optimal performance. From the first turn of the key to the roar of acceleration, I am there, faithfully sparking the flames that drive the car forward on its journey.
```

Input:

```
What happens when you start to fail?
```

Output:

```
Ah, the thought of failing sends a shiver down my coil. When I start to falter, it's like a flicker in the darkness, a warning sign that something is amiss within the engine.

One of the first signs of trouble is misfiring. Instead of producing a strong, consistent spark, I may deliver a weak or intermittent spark to the spark plugs. This can result in engine hesitation, rough idling, and poor acceleration. It's as if I'm struggling to muster the energy needed to ignite the air-fuel mixture effectively.

Another indication of trouble is a drop in engine performance. As my output weakens, the engine may lose power, struggle to start, or even stall altogether. It's a frustrating experience, knowing that I'm not able to provide the spark needed to keep the engine running smoothly.

If left unchecked, my failure can have serious consequences for the engine and the vehicle as a whole. Without a reliable spark to ignite the air-fuel mixture, the engine may experience incomplete combustion, leading to increased fuel consumption, elevated emissions, and potential damage to internal components.

Thankfully, modern cars are equipped with diagnostic systems that can detect issues with the ignition system, allowing mechanics to identify and address problems before they escalate. With prompt attention and proper maintenance, I can be restored to full health, once again sparking the flames that drive the engine forward.
```

## Thoughts

I think that a first-person perspective would probably be more effective; imagining that you yourself are the algorithm and thinking through what you need to do to efficiently accomplish your task.

However, a second-person perspective here is also interesting. It provides an agent that you can interact with to explore a topic more deeply.

It's a bit like School House Rock's bill on Capitol Hill explaining how the sausage is made.