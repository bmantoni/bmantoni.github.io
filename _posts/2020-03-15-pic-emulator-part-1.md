---
published: true
---
## Building a PIC Emulator - Part 1

### Choosing a target device - the PIC12F675
I've been curious about PIC programming and microcontrollers in general for a while and saw this as an oppportunity to learn. Initially, I thought this would be a simpler choice compared to something like an Intel 8051, given that it has "only 35 instructions to learn" and is a small 8-pin microcontroller. But it turns out they pack a lot of functionality into these little things, and being little actually creates some complexity. But, sticking with it.

### Using the XC8 Compiler to create a demo program
First, compiling a C program to the hex code that is actually programmed to the device. This same file will be the input to the emulator.
![demo-program.png]({{site.baseurl}}/media/demo-program.png)

### The C compiler outputs
The [Compiler Guide](https://ww1.microchip.com/downloads/en/DeviceDoc/52053B.pdf) explains all the files the compiler can output.

The important things for me are:
* The input is C program - a .c file
* The compiler can output an Assembly List file (.lst), with the --ASMLIST option, that is human readable and shows the addresses, hex opcodes, assembly instructions, and the original C code that it compiled from.
* The .hex file output is in the [Intel HEX](https://en.wikipedia.org/wiki/Intel_HEX) format and is what would be fed into a PIC programmer to write to the chip.

So, I'll make the emulator take the .hex file as its input.

### The instructions and opcodes
The opcodes and fields are a variable-length, depending on which instruction it is. This ends up making things quite a bit more complicated. So I'll need to match instructions based on a variable length of fixed MSBs, then parse out the remaining fields based on the instruction.

The [Data Sheet](http://ww1.microchip.com/downloads/en/DeviceDoc/41190G.pdf) explains the instruction set.

![opcodes.PNG]({{site.baseurl}}/media/opcodes.PNG)

### Memory layout
The PIC uses the Harvard Architecture, so **program and data memories are separate**. This was quite confusing at first. Further, the data memory uses **banking**, which I'd also never used in anger.

And it has an 8-level-deep hardware stack.

### Choosing a language/stack
I'm writing it in Dart, as an excuse to learn it. Maybe some Flutter later, but at first, Dart command-line

### Planning the emulator
I'll need:
* A Parser for Intel HEX to read the program and load it into memory
* A Memory data structure (program, data, stack)
* A Program Counter, pointing to the current instruction
* A CPU, that can execute all supported instructions
* I/O (later)
* A (much later) UI that lets me watch the states of the registers and GPIO "pins"

Code is here: Code is here: https://github.com/bmantoni/8bit-dart-emulator