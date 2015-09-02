---
layout: post
title: "Tunneling web traffic over SSH"
date: 2015-08-13 06:24:25 -0700
comments: true
categories: 
---

At times it is useful to have your web traffic originate from a remote location for testing or access to geofenced resources. During university it was very useful for accessing the library resources. At other times being able to have all traffic encrypted from a laptop to a remote location such as when on an unsecured WiFi network. I've used this a good number of times so here is the minimal steps.
<!-- more -->

```
ssh -C2nNqT -D 8080 dkozel@vps.derekkozel.com &
```

These options:

 * Enable compression
 * Force the use of SSH protocol version 2
 * Prevents reading of stdin, useful for running in the background
 * Does not execute a remote command
 * Disables pseudo-tty allocation
 * Quiets ssh messages

Then configure your browser to use a localhost SOCKS proxy at port 8080.
