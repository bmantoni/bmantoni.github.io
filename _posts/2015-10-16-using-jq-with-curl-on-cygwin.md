---
layout: post
splash: ""
tags: 
  - "null"
published: true
title: Using jq with curl on Cygwin
---



## Parsing JSON from HTTP requests with curl, using jq, in Cygwin

Sometimes you're just not on a Mac or Linux machine and need to work with JSON.

jq is a great tool, and if I was on my Mac and wanted to parse some JSON, retrieving it with an HTTP request, I'd just do:

{% highlight bash %}
curl 'https://api.github.com/repos/stedolan/jq/commits?per_page=5' | jq '.'
{% endhighlight %}

But on Windows, after downloading jq-win64.exe and adding it to my PATH, if I just try to pipe it, it's not happy:

{% highlight bash %}
curl 'https://api.github.com/repos/stedolan/jq/commits?per_page=5' | jq-win64.exe '.'
{% endhighlight %}
curl gets the content, but it bombs.

So, I just wrap the .exe in a sh script named jq, with this content:

{% highlight bash %}
#!/bin/bash
jq-win64.exe --color-output "$@"
{% endhighlight %}

And now it works!

{% highlight bash %}
$ curl -sS 'https://api.github.com/repos/stedolan/jq/commits?per_page=5' | jq '.[0].commit.message'
"Revert \"Adjust spacing of section headers to account for nav bar (fix #986)\"\n\nThis reverts commit 73b8413d10751c7be3e54d83ea338b3e895bdda3.\n\nThe fix for #986 caused #988."
{% endhighlight %}
