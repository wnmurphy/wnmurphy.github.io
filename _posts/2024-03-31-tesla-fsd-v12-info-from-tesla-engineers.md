---
title: Tesla's FSD v12 Information from Tesla Engineers
date: 2024-03-31T08:55:22-08:00
layout: single
permalink: /tesla-fsd-v12-info-from-tesla-engineers
category: 'technology'
tags: ['tesla', 'fsd', 'autonomy']
---

I’m a huge Tesla FSD nerd. The software fascinates me.

I got to try FSD v12.3.2.1 for the first time as a passenger yesterday, and it feels completely different in person than it does watching Youtube videos about it. It is very smooth, very human-like, and very boring. If you didn’t know it was on, you typically wouldn’t even notice as a passenger.

I just got a chance to speak with some of the engineers at the Sunnyvale showroom today. I spoke with Viraj (reliability engineering) and Jay (ADAS engineering) and they answered my questions.

My takeaways:

*Aside from the model’s weights, there is no local configuration or adjustment on the vehicle (or so they claim)*. FSD Beta testers have commonly reported that a particular release “sucks” or is worse than the prior release, and then after about a week they report that it “suddenly got better.”

This only happens to some of the beta testers, and the others are like "it's great for me, I don't know what you're talking about." This also occurs within a single geographical region, so it's not really explainable by over/underrepresentation of a location in the training data

I suspected that each dot release had multiple blind A/B versions competing for fewest interventions, that after a week they switch everyone over to the better blind version, and that this is reason it initially sucks for some testers and is amazing for others, then becomes amazing for the first group as well.

Jay told me that it’s entirely due to whatever is in the environment (input data). Basically, the perception of a change is due to latent variables + the probabilistic nature of models + the pattern-recognition tendencies of humans. In other words, there is no configuration local to the vehicle that is adjusted in any way.

However, I’m not sure I buy this. I swear I noticed a few poker tells when he understood what I was asking, so I still actually believe that this is the case. A single event noticed by an FSD Beta tester, maybe. But a trend of testers reporting the same thing? Within the same time frame from the initial release download?

*They are automatically labeling the categories of human interventions and using the label frequency to prioritize what they need to tune for next.* We kind of already knew this, but it was cool to hear confirmation. The training engine is basically automatically choosing what chunks of the problem to eat next, and they use a priority queue to maintain the order of intervention categories to tackle next.

*The remaining chunks to tackle are “last-mile.“* Things like pickup (smart summon), dropoff (banish), parking (i.e. pulling into driveways), backing up, etc. This also includes the random things that other humans do which can’t be predicted in advance, which is the real tail of the march of nines.

*The push for increasing the take rate of FSD is about ramping up the data collection.* More cars running FSD means more real-world driving data, which means more examples of edge cases, which means accelerating improvements.

Elon and Ashok mentioned that it took around 1m examples before the model started performing really well. It wasn’t clear to me whether that was 1m general driving examples in the training set, or whether that was per-case. I suspect it was the former.

With 6m Teslas on the road, and an average of 14,000 miles driven per year per car, napkin math gives us 85.6B miles annually from which to pull footage to add to the training set, scaling linearly as Tesla deliveries scale.

*Any real-world collision or safety event immediately jumps to the top of the queue.* When there’s any accident in a car, it’s immediately prioritized as the next thing that needs to be solved. This makes sense, because it’s effectively identifying a critical “bug” that requires a bug fix.

*Tesla has their own internal test drivers deployed across the country to collect data.* This is to avoid data overfit to California. We saw this with Chuck Cook's turn, when he noticed Tesla employees in vehicles repeating the turn over and over. I imagine this is also the case for any other country where FSD is available or planned.

*It requires nearly no effort to adapt FSD for a new Tesla vehicle like Cybertruck.* All they need to do is calibrate the cameras, and then programmatically adjust based on the dimensions of the new vehicle.

*It requires nearly no effort (from Tesla) to license FSD to other OEMs.* The only obstacle is on the OEM side. If Ford wants to license FSD, they need to design and build a car with computer-controllable brakes, acceleration, etc. The legacy OEMs are blocked by tight coupling with their Tier 3 and Tier 2 part suppliers. They would basically need to design a new car for it to be able to run FSD.

*They are 100% confident that “Operation Vacation” is in effect.* I asked them whether they felt they’d achieved a flywheel, and that it was now merely a matter of feeding data into the training engine. There was absolutely no doubt in their minds.

I forgot to ask about interventions-per-mile and miles-per-intervention, but it’s just as well, since I doubt they would divulge any of that to me. However, I’m certain they’re looking at giant dashboards in the office that display these metrics over time, and I belive their confidence is data-driven. They’ve got to be pretty optimistic after the v12 refactor to an imitation model.

*They are 100% confident that level 5 autonomy is a tractable (solvable) engineering problem.* Again, there was absolutely no doubt in their minds.