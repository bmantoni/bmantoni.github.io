---
published: true
---
## Building a PIC Emulator - Part 1

### Choosing a target device - the PIC12F675
I've been curious about PIC programming and microcontrollers in general for a while and saw this as an oppportunity to learn. Initially, I thought this would be a simpler choice compared to something like an Intel 8051, given that it has "only 35 instructions to learn" and is a small 8-pin microcontroller. But it turns out they pack a lot of functionality into these little things, and being little actually creates some complexity. But, sticking with it.

### Using the XC8 Compiler to create a demo program
First, compiling a C program to the hex code that is actually programmed to the device. This same file will be the input to the emulator.
![pic-demo-code.jpg]({{site.baseurl}}/media/pic-demo-code.jpg)

### Understanding the compilation inputs and outputs
The [Compiler Guide](https://ww1.microchip.com/downloads/en/DeviceDoc/52053B.pdf) explains all the files the compiler can output.

The important things for me are:
* The input is C program - a .c file
* The compiler can output an Assembly List file (.lst), with the --ASMLIST option, that is human readable and shows the addresses, hex opcodes, assembly instructions, and the original C code that it compiled from.
* The .hex file output is in the [Intel HEX](https://en.wikipedia.org/wiki/Intel_HEX) format and is what would be fed into a PIC programmer to write to the chip.

So, I'll make the emulator take the .hex file as its input.

### Understanding the assembly and HEX format
The [Data Sheet](http://ww1.microchip.com/downloads/en/DeviceDoc/41190G.pdf) explains opcode formats and instruction set.
![instruction1.png]({{site.baseurl}}/media/instruction1.png)

The opcodes and fields are a variable-length, depending on which instruction it is. This ends up making things quite a bit more complicated.



### Understanding the memory layout
**Program Memory**
Comparing the address on line 2 in the HEX file with the starting instruction address in the assembly, they don't match up. Eventually I realised that the value in the hex is exactly double. 03DB * 2 = 07B6. I think this is because hex is a 32 bit format, but it's encoding 16 bit addresses.

So, we need a data structure that can hold 1023 bytes of Program Data, and we'll load the instructions from the hex file into it.

**Data Memory (Registers) - TODO**

For registers, I need a data structure to hold 2 banks of 127 bytes each.

**Stack**

This seems complicated. It has an 8-level-deep hardware stack.

### Choosing a language/stack
I'm thinking of using Flutter and Dart, as an excuse to learn them.

### Planning the emulator
I'll need:
* A Parser for Intel HEX to read the program and load it into memory
* A Memory data structure (program, data, stack)
* A Program Counter, pointing to the current instruction
* A Microcontroller, that can execute all supported instructions
* I/O

I also want:
* A UI that lets me watch the states of the registers and GPIO "pins"
