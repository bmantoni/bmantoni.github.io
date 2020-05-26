---
published: true
---
## Building a PIC Emulator - Part 3
Code is here: https://github.com/bmantoni/8bit-dart-emulator

### Implementing the Stack

Guess what? Implement a stack:

{% highlight dart %}
class Stack<T> {
  final List<T> _stack = <T>[];
  int _max;

  Stack({int maxElements = -1}) {
    _max = maxElements;
  }

  T pop() {
    var o = _stack[0];
    _stack.removeAt(0);
    return o;
  }

  bool get hasValue => _stack.isNotEmpty;

  void push(T a) {
    if (_max >= 0 && _stack.length >= _max) {
      throw ArgumentError('Reached stack max!');
    }
    _stack.insert(0, a);
  }
}
{% endhighlight %}
  
I really like Dart. It combines things I like about C# - type inference, dynamic when you don't want to dick around, but static when you want to lock it down, extension methods. Convention over configuration with things like private members via underscores. Native support for getters. Operator overloading. Method chaining is super useful to avoid noisy lines when creating and initialising. Named constructors, optional parameters, lambdas.. lovely.

My top-level loop is now:

{% highlight dart %}
  void run() {
    while (!stop()) {
      print('Running instruction at ${HexUtilities.bytesToHex(_programCounter.address)}');
      var opcode = _memory.program.getWord(_programCounter);
      var control = _iset.run(opcode, memory);
      nextInstruction(control);
    }
  }
{% endhighlight %}
  
Which I'm pretty happy with. Get the instruction. Run the instruction. And use the result to determine the next instruction.

Next up - I haven't implemented all 35 instructions, so it's not totally complete. I'm curious about some features like the ADC, and what emulation of them might look like. Maybe I'll head there next.