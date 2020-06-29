---
published: true
---
## Using Betaflight Blackbox Logs to tune out Propwash Oscillation

Running BF 4.2 on a new F7 build, I've tweaked a few things and it flies pretty well, except for some lingering propwash oscillation. No bounceback.

So far I've bumped up the PID master ratio and the P/D balance (in favor of D). Disabled D_Min. Upped feedforward and ff transition. Enabled the RPM Filter, 0 harmonics (So, harmonics number = 1). Bidirectional DSHOT600, running at 8kHz/8kHz. Narrowed the Notch Filter Q and lowered the range since the RPM filter is doing most of that work.

Looking at the blackbox logs I see 3 things: 
1. D is rising before P in traces during propwash, making me think my problem is with the D term amplifying the noise. 
2. In the spectral analysis, the noise peaks falls off above the D-term Lowpass 1 (PT1) filter, and too far below the D-term LPF2 cutoff.
3. There are very regular spikes on the low end. And if I count them, it looks like there's 16 of them.

![June29-bbl-trace-freq.PNG]({{site.baseurl}}/media/June29-bbl-trace-freq.PNG)

So I'm going to raise the LPF1 filter range from 98-238Hz, to 115-275Hz.

And lower the LPF2 filter cutoff from 210 to 190.

And those low-end spikes look like a multiple of the 4 motors. So increasing to 2 harmonics on the RPM filter).

![June29-bbl-trace-PIDs.PNG]({{site.baseurl}}/media/June29-bbl-trace-PIDs.PNG)

To test this, I'm curious if this early rise in D goes away. If not, and if the noise peaks fall in into the filter ranges better then I'll try lowering P gain and/or raising D gain.
