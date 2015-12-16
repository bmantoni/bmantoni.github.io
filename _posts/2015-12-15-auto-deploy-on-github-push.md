---
layout: post
categories: [raspberry pi]
tags: [raspberry pi, diy, python]
published: true
title: Auto deploying code on push to Github
---

## Auto deploying code to a Pi when pushes to Github

I have my Pi controlling my Hue lights (see other project) and wanted to streamline the deployment of updates to the python script controlling it.

I wanted to be able to code on a regular machine, push to GitHub, and have that code automatically deploy to the Pi, so I don't have to ssh to it and pull.

 * Github Webhooks make this possible. Set one up in my repo to fire on pushes, to make an HTTP POST to URL I'll host on the Pi.
 * Use Flask to host a lightweight web server
 * Run the flask as a daemon on the Pi, as a restricted user
 * Invoke commands in the Flask handler to stop and start the daemon running the script getting the updates, and do a `git pull`

This is working great, made my development workflow much more efficient, and definitely impressed upon me once again how flexible a Pi is. Mine is now making JSON/HTTP requests out as a client, and acting as a server to incoming HTTP requests.

But there is room for improvement. Webooks support message-based authentication that I haven't implemented, and supporting SSL would be a good idea, though I'd probably want to offload that to another more powerful server somewhere on my network. But I see the risk here as pretty low since I can restrict source IPs to Github's.

[Source Code on Github](https://github.com/bmantoni/auto-deploy-github-hook)
