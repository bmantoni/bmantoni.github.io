---
layout: post
splash: ""
tags: 
  - "null"
published: true
title: Taking a dump
---


## Simple example of taking a (heap) dump and analyzing it

I just wanted to prove that I can take a dump, and see something meaningful in it. To do this I wrote a simple program that creates an obvious memory usage pattern that should be dead-simple to see in the dump.

Compile and run the following code. It will intentionally create an array with many Integers. 

{% highlight java %}
public class DumpTest
{
	public static void main(String[] args) throws Exception
	{	
		final Integer NUM_FACTS = 100000;
		Foo[] x = new Foo[NUM_FACTS];
		
		for(int i = 0; i < NUM_FACTS; i++)
			x[i] = new DumpTest().new Foo();
		
		System.out.println("Populated array with " + NUM_FACTS.toString() + ". Waiting 10 seconds...");
		Thread.sleep(256000);
		System.out.println("Done.");
	}
	
	public class Foo
	{
		public int x;
	}
}
{% endhighlight %}

The code conveniently waits after it has created all these objects, so we can take the dump while it's waiting:

{% highlight bash %}
jmap -dump:format=b,file=heap.bin 8216
{% endhighlight %}

Open it up in VisualVM, and there we have it - many, many Integers.
