---
title: CLI Management with Navi
date: 2019-11-25T12:24:41-08:00
layout: single
category: 'tools'
tags: ['cli']
---

If you're sick of looking through your shell history to find a command to modify and reuse, check out [Navi](https://github.com/denisidoro/navi).

Navi is an "interactive cheatsheet tool for the command-line." You create lists of commands (called "cheats") you want quick access to. These lists can be grouped by concern (git, ssh, etc.). You can also define the variables within each command. 

When you need a command, run `navi` and start typing a partial command or the description you wrote in your command list. Select the command, and if it has any variables defined Navi will prompt you for those values.

Navi basically allows you to create easy-access templates for the commands you use most often. A good use case is to add anything you've had to look up more than once in the last month.

Navi comes with 19 pre-built lists of commands (git, docker, kubernetes, mysql, etc.)

![Navi example](https://user-images.githubusercontent.com/3226564/67864139-ebbcbf80-fb03-11e9-9abb-8e6664f77915.gif)

The only thing I dislike about Navi is that the command doesn't also result in a `history` entry, but this is something that could probably be easily patched.