---
published: true
---
## Building a PIC Emulator - Part 2
[Part 1 is here](http://bmantoni.github.io//pic-emulator-part-1/)

[Part 3 is here](http://bmantoni.github.io//pic-emulator-part-3/)

Code is here: https://github.com/bmantoni/8bit-dart-emulator

### Class Design

![PIC Emulator Classes.png]({{site.baseurl}}/media/PIC Emulator Classes.png)

The HEX parser and program runner were pretty straightforward. I parse the hex and extract the 2-byte words, loading them into a Program Memory. A Program Counter (Address) is used as a pointer to the current instruction, and it's incremented from 0 until it finds an instruction it recognises. The challenge came when I tried to figure out how to increment it.

The PIC uses 14-bit opcodes, and uses word addressing. So **Program Memory** Address 0x2 is actually byte address 0x4, since each word is stored in 2 bytes. I hide this in a ProgramMemory class.

**Data Memory** is separate, since PICs use Harvard Architecture, and uses normal byte addressing. The complexity here comes from the memory banking, which is switched using a bit at address 0x5, the Status register. I abstract this using an interface that the Memory classes implement.

I wanted each instruction implementation to be as minimal as possible - with each Instruction as a class that only has the behaviours that differ per instruction.

So a simple instruction like movwf can be implemented like this:

{% highlight dart %}
// move W to f
class MovWf extends Instruction {
  @override
  Fields extractFields(ByteData opcode) {
    return Fields(f: extractField(opcode, 7, 7));
  }

  @override
  int get mask => 1;

  @override
  int get offset => 7;

  @override
  Instructions get name => Instructions.movwf;

  @override
  Function(Fields, Memory) get runFunc => (f, m) => m.data.setByte(f.f, m.w);
}
{% endhighlight %}

Each instruction needs to extract the fields it uses from the opcode, define the MSB bits that identify it, have a name, and (optionally) implement a function that it performs on memory/registers. Some instructions affect control flow, so they can optionally implement a second function to say how the next instruction should be chosen. For example, this is goto:

{% highlight dart %}
class Goto extends Instruction {
  @override
  Fields extractFields(ByteData opcode) {
    return Fields(k: extractField(opcode, 11, 11));
  }

  @override
  int get mask => 5;

  @override
  int get offset => 11;

  @override
  Instructions get name => Instructions.goto;

  @override
  ControlFlow Function(Fields, Memory) get controlFunc => 
    (f, m) => ControlFlow(goto: f.k);
}
{% endhighlight %}

Next up:
* Implement the Stack and call
* Addition, subtraction, etc...
