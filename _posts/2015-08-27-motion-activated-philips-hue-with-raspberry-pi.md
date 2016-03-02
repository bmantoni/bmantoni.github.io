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

Hue lights are great. The flexibility of having so many options for colors and brightness, and to be able to control multiple lights in different rooms is great. I wanted to use it in my nursery so we could have deep red light at low brightness for middle-of-the-night changes, but brighter warm light for the mornings, and something fun during the day. 

However, one very practical problem is - when you're carrying an infant in both hands at 3am in the dark, finding your smartphone, unlocking it, launching an app, and clicking through to start the scen you want is not very convenient. 

This is actually a problem in general I think. Unlocking a phone and getting to the app can just take too much time in many cases. It almost makes you wish there was just a switch on the wall.

So, I knew I wanted to motion control the thing. I wanted this to happen automatically when I walk into the room - I want the lights to just turn on. When I'm done and I leave, turn off. We can also use the time of day to determine what specific colors and brightness for each bulb. 

I accomplished this with a Raspberry Pi, a motion control sensor, and some Python.

TODO: add link

[Source Code on Github](https://github.com/bmantoni/pi-hue-motion)
