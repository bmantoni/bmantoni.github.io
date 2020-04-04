---
published: false
---
## Building a PIC Emulator - Part 2

### Class Design

![PIC Emulator Classes.png]({{site.baseurl}}/media/PIC Emulator Classes.png)

The HEX parser and program runner were pretty straightforward. I parse the hex and extract the 2-byte words, loading them into a Program Memory. A Program Counter (Address) is used as a pointer to the current instruction, and it's incremented from 0 until it finds an instruction it recognises. The challenge came when I tried to figure out how to increment it.

The PIC uses 14-bit opcodes, and uses word addressing. So Program Memory Address 0x2 is actually byte address 0x4, since each word is stored in 2 bytes. I hide this in a ProgramMemory class.

Data Memory is separate, and uses normal byte addressing. There complexity here comes from the memory banking, which is switched using a bit at address 0x5, the Status register.

### Using the XC8 Compiler to create a demo program
First, compiling a C program to the hex code that is actually programmed to the device. This same file will be the input to the emulator.
![demo-program.png]({{site.baseurl}}/media/demo-program.png)

### The C compiler outputs
The [Compiler Guide](https://ww1.microchip.com/downloads/en/DeviceDoc/52053B.pdf) explains all the files the compiler can output.
