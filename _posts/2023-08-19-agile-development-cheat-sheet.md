---
title: Cheat sheet to Agile software development

date: 2023-08-14T21:09:22-08:00
layout: single
permalink: /agile-software-development-cheat-sheet/
category: 'organizations'
tags: ['agile']
---

This is a cheat sheet for anyone from a junior dev or non-technical background joining a technical team for the first time, who wants to know how it typically works and what to expect.

## Overview 

Agile is the gold standard for software development. Almost every company that produces software uses Agile.

The Agile paradigm is about autonomous, self-directed teams that define, estimate, and plan their own work. In Agile, new features and improvements are continuously released.

This is a change from the prior “waterfall” paradigm, where deadlines are externally imposed, teams can’t adapt to schedule slippage, and the entire project is potentially threatened if any team’s part runs late.

Agile teams:
* add roughly defined tickets (representing problems to solve) to the team’s **backlog**.
* regularly prioritize and review the upcoming tickets during **backlog grooming**, thoroughly discussing until the scope is clear, brainstorming potential solutions, and adding more definition to the tickets.
* collectively estimate, plan, and assign the next 2-week interval of work (called a **sprint**) during **sprint planning**.
* start the sprint and perform the work they planned for the sprint.
* demo what they built for a broader audience at the end of the sprint.
* review their own performance at the end of the sprint.
* repeat.

## Team composition

An Agile team typically has:

* **Product Owner.**
  * Advocates for the users/stakeholders.
  * Interviews users/stakeholders to identify their pain points.
  * Brings work to the team in the form of problem statements for the team to solve.
  * Adds these to the team’s backlog.
    
* **Engineering Manager/Technical Lead.**
  * Advocates for the engineers.
  * Should be present at all higher-level product discussions to represent the engineers before any work trickles down to them via the backlog.
   * Starts thinking early about possible approaches, what features can be reused, what features can be extended, what features will need to be built from scratch, and general approaches.
     
* **Scrum Master**.
  * Advocates for organization and coordination of Agile processes, like creating/managing/assigning tickets, running the sprint ceremonies, etc.
  * This may be a separate role, or it may just be done by the engineering manager/technical lead.
    
* **UX/UI Designer.**
  * Advocates for the user’s experience.
  * Creates UI mockups for engineers to use as guidance.
  * Mockups serve as a starting point for more engineering discussion.
    
* **Engineers.**
  * Advocates for a clean, maintanable code base.
  * Should be asking questions until they fully understand the problem they’re being asked to solve.
  * Should brainstorm solutions and make the tradeoffs clear to the Product Owner, who then chooses which solution to implement based on business priorities.
    
* **QA engineer.**
  * Advocates for the quality of the code.
  * Rigorously tests the engineer’s code. Tries to break it.
  * Identifies bugs and creates bug tickets, assigned to the engineer to fix.
  * Gives the final say that a feature is ready for deployment to Production.

## Sprint ceremonies (recurring meetings)

### Backlog grooming 

The purpose of Backlog Grooming is to keep a healthy queue of work ready to pull in during planning for the next sprint.

The most important part of Backlog Grooming is that the team fully understands the problem that each ticket is meant to solve.

During Backlog Grooming, the team reviews tickets in the backlog to understand the scope, and prioritizes them according to business needs.

Each ticket is discussed in an open-ended way, engineers ask questions, identify edge cases, and start brainstorming possible solutions.

Existing solutions should be reused. If not possible, existing solutions should be extended. If not possible, only then should new solutions be considered.

Engineers should brainstorm solutions and make the tradeoffs clear to the Product Owner and Engineering Manager/Tech Lead.

A ticket is not ready to be groomed if:
* It’s not clear what the problem is to be solved.
* It’s not clear who needs that problem to be solved, or why.
* It’s not clear that the problem is actually a problem that needs to be solved.

If any of the above are the case, then the Product Owner should clarify, or take an action item to get more information.

The team brainstorms solutions, and a general approach is sketched out in the ticket. 

Backlog grooming usually occurs once per sprint, but can be weekly if the discussions run long.

### Sprint planning

The team decides what tickets to pull in from the now-groomed-and-prioritized backlog. 

The team assigns point estimates to tickets. This is often done with sprint poker (see below), but sometimes a single engineer familiar with the topic just makes an estimate. 

Tickets are assigned to engineers. 

At the end of the meeting, the scrum master looks at the number of points for each engineer, to make sure that work is evenly distributed. No engineer should have more than 1 sprint’s worth of work, which is typically 13 points (see below).

#### Sprint points

Sprint points are estimates of the relative complexity of a ticket. Sprint points are intentionally vague. You’re not supposed to, but teams almost always convert them into time.

Fibonacci numbers are used, so that the bigger a chunk of work is, the size of the number increases in a non-linear way. This is because as complexity increases, the number of unknown variables increases. 

Sprint points explanation:

* 1 point: “Maybe an hour or two.”
* 2 points: “Maybe half a day.”
* 3 points: “Maybe a day.”
* 5 points: “Probably like 2 days.”
* 8 points: “Probably a week.”
* 13 points: “Two weeks, the entire sprint.”
* 21 points: “This ticket is too big to be actionable, and needs to be broken into smaller chunks.”

Sometimes, a team will redefine a sprint to be 8 points, rather than 13. They may also define a sprint to be 1 week rather than 2. This is just confusing and not recommended.

#### Sprint poker

Sprint poker can be done in a Zoom chat, or you can use a free web app for “planning poker.”

During sprint planning, for each groomed ticket (meaning the entire team was introduced to the ticket during Backlog Grooming, thoroughly discussed it, and understands the scope), the scrum master tells everyone on the team to secretly choose a point estimate for that ticket (see above).

On the count of three, there’s a “showdown” like in poker, and everyone’s estimates are revealed. Anyone who made an outlier estimate is invited to explain why they think the ticket is more/less work.

Sometimes they point out something no one thought of. Other times, they agree that their estimate was a bit high and can come down a bit.

The general consensus estimate gets added to the ticket.

If you don’t have a “sprint poker” app, you can just invite everyone to put their estimates in the meeting’s chat.

Sprint poker is kind of fun.

### Daily standup

Daily check-in, no more than 15 minutes. Should be quick.

The purpose is to hold everyone accountable by asking them to provide a short update. The other purpose is to identify blockers.

Everyone goes around, in alphabetical order, or by function, etc. and gives a _short_ update:

* Yesterday, I...
* Today, I’ll...
* No blockers. (or “I’m stuck until I get confirmation from Ops,” etc.)

If a team member is blocked, the Scrum Master facilitates the unblocking by setting up a meeting, taking issues offline as action items, etc.

If quick updates turn into a deeper discussion, the Scrum Master should identify this as a topic for afterwards and redirect back to updates.

### Sprint review/demos

A fun meeting, often involving multiple teams (ours is the entire engineering division), where engineers volunteer to demo the features they’ve just built in that sprint.

Very casual, ideally you demo features that are finished, but it’s fine if things are still a bit rough.

Lots of fun to see what people built and what other teams are working on.

### Sprint retro

The team meets at the end of the sprint to review how it went. 

This is typically organized on a Kanban board/Trello with columns for discussion, managed by the Scrum Master.

Example columns 1:
* What should we keep doing?
* What should we stop doing?
* What should we start doing?

Example columns 2:
* What went well?
* What didn’t go well?
* Open questions
* Action items

The team is free to add retro items throughout the week as they occur. During Sprint Retro, everyone gets 5-10 minutes to brainstorm and add items to the columns. Then the team reviews each item and discusses.

Transparency is key, and honesty, and not criticizing/blaming.

Emoting/venting is totally fine during retro.


## Typical sprint schedule

Week 1:
* Monday
  * Daily Standup
  * Sprint planning + start the sprint
* Tuesday
  * Daily Standup
* Wednesday
  * Daily Standup
* Thursday
  * Daily Standup
* Friday
  * Daily Standup
Week 2:
* Monday
  * Daily Standup
* Tuesday
  * Daily Standup
* Wednesday
  * Daily Standup
* Thursday
  * Daily Standup
  * Backlog grooming
* Friday
  * Daily Standup
  * Sprint Review/Demos (all eng teams)
  * Sprint Retro (just your team)
 
## Summary

Agile involves the continual addition of work into a team-managed queue called the backlog. 

This backlog is regularly groomed, where the team discusses the work and considers possible solutions.

During sprint planning, the groomed tickets are assigned points by consensus, and assigned to individual engineers to work on in the next sprint.

At the end of the sprint, the work is demoed to the organization during sprint review.

The team has it’s own internal retrospective, where kudos are given, issues are raised, process improvements are identified, etc.



## Bonus material: how to speak to engineers

Lingo:
* **smell**/**code smell**. A smell is anything that starts to look like an anti-pattern or bad idea. Kind of another word for “red flag.” “I’m not sure we should be committing to work before we understand it; that’s a major smell.”
* **local**. Your “local machine,” your laptop.
* **repro**. Short for “reproduce.” “I wasn’t able to repro that bug on my local, are you sure you’re running the latest version?” “Can confirm, I was able to repro on my local; it’s definitely a bug.”
* **context-switching**. Having to switch to another activity when you were already focused on one. 
* **cognitive load**. How hard it is to understand something. Complexity is a smell, because it increases the cognitive load required to read something, write code for something, or understand the code you wrote a month earlier.

Things engineers love:
* **Don’t tell them what to build.** Tell them the problem to solve and let them figure out what to build to solve it.
* **Protect their time.** Heads-down time is sacred, because it’s hard to enter the flow state required to write software. Context-switching is the bane of an engineer’s existence. It’s why we hate meetings. Any interruption can break the flow state, and it takes another 20-30 minutes to get back after an interruption. Batch your questions for an engineer, and then set up a 10-minute Zoom meeting to review them all at once. This way they’re not being peppered with interruptions.
* **Protect them from unnecessary work.** The feature you don’t build costs absolutely nothing to maintain, requires no documentation or unit tests, can’t have bugs, etc. The best feature is no feature. Rework/wasted work is a giant pain in the ass, because it’s not just the time it took to write the code; it’s the research time, the design time, the documentation time, the time spent writing unit tests to verify your code, the time QA wastes verifying your code, etc. The absolute most satisfying thing to do with a Jira ticket is to change the status to _Won’t Do_.
* **Simplicity**. Complexity is like rot when it comes to a codebase, or even an organization. If it can’t be explained simply, it’s too complex. Anything that feels too complex should be an instant smell. Good solutions are simple.
* **Ask effective questions.** “I want to X. I’ve tried Y1 and Y2. When I do, I observe that Z1, but I expected Z2. Do you know why that could be?” The first part immediately narrows the scope of the question to a particular domain. The second tells them what you’ve already done, and the third tells them the exact symptoms you’re seeing.