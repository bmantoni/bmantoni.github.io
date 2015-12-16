---
layout: post
categories: [raspberry pi]
tags: [raspberry pi, diy, python]
published: true
title: Motion Activated Philips Hue with Raspberry Pi
---

## Controlling Hue lights with a Pi and Python over wifi

![intro3.jpg]({{site.baseurl}}media/intro3.jpg)

**TODO - embed youtube video** 

I finally found an excuse to go out and buy a Hue kit when my first baby arrived. I want the nursery to offer a range of interesting sensory stimuli for his little developing mind, especially visual and auditory. A Bluetooth speaker and Spotify handle the latter easily enough, and this clever technology from Philips - with it's beautiful range of saturated, luminous treats - can engage the retinas and, I hope, be a practical solution to some more mundane problems.

After setting up the out-of-the-box kit and experimenting with a few apps, I found the experience quite pleasing (and so did he). I could run a lower-intensity scene for 3am changes, psychedelic animated color progressions when he's awake, and even have the lights sync'd up with music. I found the Hue Disco app is able to do it all, though there are many options.

However, one very practical problem is - when you're carrying an infant in both hands at 3am in the dark, finding your smartphone, unlocking it, launching an app, and clicking through to start the scen you want is not very convenient. It almost makes you wish there was just a switch on the wall.

I wanted this to happen automatically. When I walk into the room I want the lights to just turn on. When I'm done and I leave, turn off. But how?

[Source Code on Github](https://github.com/bmantoni/pi-hue-motion)
