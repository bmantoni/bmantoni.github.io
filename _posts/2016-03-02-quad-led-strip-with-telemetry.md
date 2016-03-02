---
layout: post
categories: [quad]
tags: [diy, quad]
published: true
title: How to Use LED Strips with Telemetry Using Hardware Serial
---

## How to Use LED Strips with Telemetry Using Hardware Serial

I had gotten telemetry working (for battery voltage) with my Dragonfly32 (from Multirotormania) + FrSky D4R-II (+ Taranis) - working great reading Vfas.

Next I wanted to get some LEDs going - and I did get them to work today, but only after turning off softserial. I was only able to get telemetry working with softserial (see link and MRM).

With softserial off I don't get anything in the Vfas variable. Eventually I found this comment online:

"Since RC5 is also used for SoftSerial on the Naze/Olimexino it means that you cannot use SoftSerial and led strips at the same time." - LedStrip

Even though I'm using pin 6 for telemetry, and 5 for the LEDs.

So, the answer is to use a hardware serial port instead of SoftSerial. But doing so requires a new little component:

 * [FUL-01 Serial Inverter](http://www.hobbyking.com/hobbyking/store/uh_viewItem.asp?idProduct=57104)

My wiring looks like:

![wiring diagram](/media/hwserial-inverter-wiring.jpg)

The lead with the X (pin 6) is removed.

Hardware:

 * Cut and soldered 3 wires from a servo lead to ful01 as in diagram
 * Attached/soldered pin from ful01 to frsky (telemetry Rx)

In Cleanflight:

 * set telemetry_inversion = 0
 * Ensured softserial disabled and telemetry enabled.
 * On Ports tab, set USBUART Telemetry to FrSky (was Disabled)

That got me my telemetry values back on my Taranis (Vfas, Cell, Cells), and I can enable LED_STRIP in CF!
