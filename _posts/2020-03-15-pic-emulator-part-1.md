---
published: false
---

## Building a PIC Emulator - Part 1

### Choosing a target device - the PIC12F675
Choosing this as there are only 35 instructions to learn, it's a simple micro, and I've been curious about PIC programming for a while.

### Using the XC8 Compiler to create a demo program
![Capture.PNG]({{site.baseurl}}/_posts/Capture.PNG)

### Understanding the compilation inputs and outputs
The [Compiler Guide](https://ww1.microchip.com/downloads/en/DeviceDoc/52053B.pdf) explains all the files the compiler can output.

### Understanding the instruction set
The [Data Sheet](http://ww1.microchip.com/downloads/en/DeviceDoc/41190G.pdf) explains the Instruction Set.

### Understanding the memory layout

### Choosing a language/stack
I'm thinking of using Flutter and Dart, as an excuse to learn them.

### Planning the emulator
I'll need:
* A Parser to read the program and load it into memory
* A Memory data structure
* A Program Counter, pointing to the current instruction
* A Microcontroller, that can execute all supported instructions
* I/O

I also want:
* A UI that lets me watch the states of the registers and GPIO "pins"