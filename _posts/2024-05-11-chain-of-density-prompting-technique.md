---
title: Chain of Density prompting technique
date: 2024-05-11T07:10:22-08:00
layout: single
permalink: /chain-of-density-prompting-technique/
category: 'programming'
tags: ['llm', 'prompts']
---

I read a whitepaper on this [Chain of Density prompting technique](https://arxiv.org/pdf/2309.04269) a while ago that fascinated me.

You can essentially use an LLM to simulate a recursive summarizing operation, where each subsequent summary is more information dense than the prior ones.

The trick to this is the same as Chain of Thought techniques: you need to give the model an information space to write its thoughts, like a scratch pad.

Here, a JSON array is used, where each element is an object that serves as a working memory for the operation of generating subsequent objects.

## Prompt

```
Article: $ARTICLE

You will generate increasingly concise, entity-dense summaries of the above Article.
Repeat the following 2 steps 5 times.

Step 1. Identify 1-3 informative Entities ("; " delimited) from the Article which are missing from the previously generated summary.

Step 2. Write a new, denser summary of identical length which covers every entity and detail from the previous summary plus the Missing Entities.

IMPORTANT: each subsequent summary must include every entity and detail from the prior summary AND any Missing Entities.

A Missing Entity is:
- Relevant: to the main story.
- Specific: descriptive yet concise (5 words or fewer).
- Novel: not in the previous summary.
- Faithful: present in the Article.
- Anywhere: located anywhere in the Article.

Guidelines:
- The first summary should be long (4-5 sentences, ~80 words) yet highly non-specific, containing little information beyond the entities marked as missing. Use overly verbose language and fillers (e.g., "this article discusses") to reach ~80 words.
- Make every word count: re-write the previous summary to improve flow and make space for additional entities.
- Make space with fusion, compression, and removal of uninformative phrases like "the article discusses"
- The summaries should become highly dense and concise yet self-contained, e.g., easily understood without the Article.
- Missing entities can appear anywhere in the new summary.
- Never drop entities from the previous summary. If space cannot be made, add fewer new entities.

Remember, use the exact same number of words for each summary.

Answer in JSON. The JSON should be a list (length 5) of dictionaries whose keys are
"Missing_Entities" and "Denser_Summary"
```


I've modified the prompt from the paper to include an emphasis on these instructions, which I found were being ignored with GPT-3.5:

`IMPORTANT: each subsequent summary must include every entity and detail from the prior summary AND any Missing Entities.`

## Input article

Here's an article I grabbed from [Scientific American](https://www.scientificamerican.com/article/why-did-ancient-romans-make-this-baffling-metal-dodecahedron/)...

```
Roman dodecahedra are something of an enigma: there is no known mention of these 12-sided, hollow objects in ancient Roman texts or images. First discovered in the 18th century, around 130 dodecahedra have been found across the Roman Empire, although it is interesting that the majority have been found in northern Europe and Britain, and none have been found in Italy.

Dodecahedra are quite intricate, featuring a number of round holes, with knobs framing the holes. It would have taken a very skilled craftsman to make them. They are made out of a copper alloy and would have been quite expensive, due to the amount of time and metal that was used to create them, which adds to their intrigue.

I am part of the local archaeology group behind the recent discovery of a Roman dodecahedron in Norton Disney, near Lincoln. It has been quite a whirlwind for our group, from the shock on the day of finding the object, where everyone on site was buzzing with excitement and disbelief, to dealing with all the attention both nationally and internationally. It has been wonderful to witness the interest in our find and the history of Norton Disney.

There have been numerous suggestions by archaeologists and the public as to what dodecahedra could have been. Some theorise that they were religious objects, knitting tools, measuring instruments or stress toys. Due to the high level of skill involved, some have suggested that they were a way for a master craftsman to demonstrate their expert abilities.

There is no uniformity in the size or shape of the dodecahedra found so far, nor in their metal composition or even in the level of craftsmanship. If they were important objects, we would expect to also discover contextual evidence in the archaeological record, such as depictions in paintings or mosaics. It does feel that this object will remain a mystery for some time – which might be why so many people are fascinated by it.

Four trenches were opened, and it was in trench four – in what appeared to be a large pit – that a perfectly crafted dodecahedron was found. It’s the 33rd to be found in England and the first to be found in the Midlands.

There are a few things that make this find particularly special. First is its size, as it is thought to be one of the largest examples in Britain. Second is the high level of preservation of the object. As Richard Parker, the secretary for the NDAG, explained: “Ours is in absolutely fantastic condition. It is completely undamaged and there is no evidence of any wear at all.”

The dodecahedron has undergone some initial analysis in order to try to provide some more clues about it. A handheld XRF (X-Ray flourescence) analysis, a technique used to analyse element composition, was carried out by archaeometallurgist Gerry McDonnell, an expert in the past use and production of metals by humans. It revealed that the composition of the object was mostly a mix of copper alloy (75%), with tin (7%) and lead (18%).

The Norton Disney dodecahedron measures around 8cm across and weighs 245 grams. It has also been scanned using a 3D scanner in collaboration with the University of Lincoln, and later this year it will be sent to Newcastle University for some further scientific analysis.

The site of the find itself is interesting. Pottery shards from a number of the trenches ranged in date from the Iron Age up to the Roman period, showing a long, continued use of the land.

There is also a Roman Villa close to the site that was excavated in 1935. Skeletal remains found at the villa suggest that it was occupied in the late Roman period, and that the villa site was later reused as a burial ground. In 1989, a metal detectorist discovered a Romano-British horseman deity figurinein the vicinity of the Roman villa, which is currently housed at the British Museum.

There is still so much to learn about the site and the dodecahedron itself. The trench where it was found was not fully excavated in 2023 due to time and financial constraints (the NDAG is solely reliant on donations), as it was found on the penultimate day of the dig.

But the NDAG will be returning to the site this June to reopen a couple of trenches and fully excavate the pit where the dodecahedron was found. This will hopefully provide a better picture of exactly what the site was used for and why the mysterious dodecahedron was placed there.
```

## Result

```
[
    {
        "Missing_Entities": "Norton Disney, Lincoln",
        "Denser_Summary": "This article discusses the recent discovery of a Roman dodecahedron in Norton Disney, near Lincoln. The dodecahedron, found in trench four, is well-preserved and one of the largest in Britain. Initial analysis reveals its composition: mostly copper alloy with tin and lead."
    },
    {
        "Missing_Entities": "archaeometallurgist Gerry McDonnell",
        "Denser_Summary": "This article discusses the recent discovery of a Roman dodecahedron in Norton Disney, near Lincoln. It was found in trench four and analyzed by archaeometallurgist Gerry McDonnell. The object, well-preserved and intricate, is thought to have had multiple potential uses."
    },
    {
        "Missing_Entities": "University of Lincoln, Newcastle University",
        "Denser_Summary": "This article discusses the recent discovery of a Roman dodecahedron in Norton Disney, near Lincoln. The object underwent analysis by archaeometallurgist Gerry McDonnell. It will undergo further study at the University of Lincoln and Newcastle University."
    },
    {
        "Missing_Entities": "Iron Age, Roman period, NDAG",
        "Denser_Summary": "The Roman dodecahedron, found near Lincoln, underwent analysis by archaeometallurgist Gerry McDonnell. It will be further studied at the University of Lincoln and Newcastle University. Pottery shards spanning from the Iron Age to the Roman period were found at the site."
    },
    {
        "Missing_Entities": "late Roman period, British Museum",
        "Denser_Summary": "The Roman dodecahedron, found near Lincoln, underwent analysis by archaeometallurgist Gerry McDonnell. Pottery shards from the Iron Age to the Roman period were discovered at the site. Skeletal remains suggest late Roman occupation, and a nearby figurine is housed at the British Museum."
    }
]
```

You can see how each new `Denser_Summary` enriches the prior one with the `Missing_Entities`.

## Discussion

An "information space" is any place to put information, like CPU, or RAM, or a database. The issue with vanilla LLMs is that they have no place to put additional information during inference by default.

Adding an information space to your output gives the model a scratch pad to write intermediate data, analogous to how we reason and plan internally in our working memory when someone asks us to complete a task.

Models are typically trained on JSON data, which can also be used to represent serial iterations in the form of a JSON array, and the objects in that array can be made to store a wide variety of intermediate data..

Aside from being a really cool summarizing technique, this is a solid illustration of what can be done with a single prompt if you give the model information space to "think."