---
published: false
---
## Building a PIC Emulator - Part 4

[Part 1 is here](http://bmantoni.github.io//pic-emulator-part-1/)

[Part 2 is here](http://bmantoni.github.io//pic-emulator-part-2/)

[Part 3 is here](http://bmantoni.github.io//pic-emulator-part-3/)

[Code is here](https://github.com/bmantoni/8bit-dart-emulator)

I finally created a little UI to visualise the emulator working. This let me actually use Dart for a Flutter project, and reference in the emulator package created in the previous posts.

![emu-ui.PNG]({{site.baseurl}}/media/emu-ui.PNG)

It's bare-bones, but this is all I had in mind - to see the memory and how the PC moves through it. You can see in the test program that the way it implements a wait is to just bounce around for a certain amount of time.

You can change the amount of memory that's in view to avoid scrolling, and change the speed of the emulator. I had to add some hooks into the emulator project, since Flutter is single-threaded, I had to make the emulator instructions run asynchronously, so the UI can update in between.

It's pretty cool that the same code I was running as a CLI app can now be used in an Android or iOS app, or webapp, with no changes, and look native in all 3.
