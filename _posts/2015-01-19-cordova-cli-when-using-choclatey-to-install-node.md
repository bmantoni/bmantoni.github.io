---
layout: post
category: blog
published: true
title: Cordova CLI when Using Choclatey to Install Node
---

## Setting Path for Cordova CLI when using Choclatey on Windows

If you use Choclatey to install node (and then do npm install -g cordova) and try to follow the Phonegap/Cordova instructions, you'll hit a wall when you try to run the cordova command. The problem is here:
http://docs.phonegap.com/en/edge/guide_cli_index.md.html#The%20Command-Line%20Interface
> You may need to add the npm directory to your PATH in order to invoke globally installed npm modules. On Windows, npm can usually be found at C:\Users\username\AppData\Roaming\npm.

The reason is that choclatey doesn't install node where it would go if you installed it directly. Rather (by default) it puts things under C:\ProgramData\choclatey

So if you add the above to your path, that's not where cordova will be and you'll get an error that the executable can't be found.

So, to find the correct path, use this:
> PS > npm config get prefix
C:\ProgramData\chocolatey\lib\nodejs.commandline.0.10.35\tools

In that directory you should find a file, cordova.cmd.

So, add that directory to your path and you should be good to go!