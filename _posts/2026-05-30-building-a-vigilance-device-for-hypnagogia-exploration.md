---
title: Building a vigilance device for hypnagogia exploration 
date: 2026-05-30T10:32:22-7:00
layout: single
permalink: /building-a-vigilance-device-for-hypnagogia-exploration/
category: 'programming'
tags: ['psychology', 'projects']
---

For a while now, I've wanted to build a [vigilance device](https://en.wikipedia.org/wiki/Dead_man%27s_switch) for exploring [hypnagogia](https://en.wikipedia.org/wiki/Hypnagogia), the pre-dream state where the body is relaxed and hypnagogic imagery appears.

A handful of prominent figures have used a "drop technique" for this: Salvador Dali, Albert Einstein, Nikolai Tesla, Thomas Edison. Each one would sit or recline for a nap, close their eyes, and hold an object like a metal spoon, key, or ball bearings over a metal plate on the floor. As the body relaxes, hypnagogic imagery appears. As they drift off to sleep, further relaxation causes them drop the object, waking them up. Rinse and repeat.

I ordered some simple electronic components to build a device like a remote control, where you can just press and hold a normally-open button, such that releasing the button causes a pager motor to vibrate, stirring you from sleep. However, the enclosure was taking too long to ship.


![8BitDo SN30 Game Controller]({{ '/assets/images/8bitdo-sn30.webp' | relative_url }})

I realized that I could repurpose an [8BitDo bluetooth game controller](https://www.amazon.com/Bluetooth-Controller-Compatible-Raspberry-Gaming-Console/dp/B0CSPCSTV2) and write some python to accomplish the same thing, and it's been totally effective.

This is the [repo](https://www.github.com/wnmurphy/vigilance_device) for the python script. There's a "press-and-hold" button, where releasing it causes the controller to vibrate. There's also a "disable monitoring" button, which you can press to pause/turn off the action of the first button.

You can customize which buttons to use (the shoulder triggers have a lighter action, for example), and experiment with the grip. Pair your controller, start the script, go lay down or sit somewhere and start watching the movie theater on the back of your eyelids as your [secondary visual cortex](https://magazine.hms.harvard.edu/articles/behind-veil-hypnagogic-sleep) takes over. Enjoy!