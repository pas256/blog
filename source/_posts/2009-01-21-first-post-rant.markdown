---
layout: post
title: "First Post – rant"
date: 2009-01-21 17:55
comments: true
categories:
- first
- rant
- sys-admin
---
Well, this is my first blog post, so what should I write about? Why not have a little rant about something I discovered recently (at least on Ubuntu).

`sudo` depends on DNS

WTF? Why does something like local privilege escalation, which does not leave the machine I am on, have anything to do with networking. Further, why the hell should a network configuration issue stop sudo from working. And even further still, why would Ubuntu (which as part of the normal install process does not set a root password) allow something as essential and necessary as sudo to be depended on a functioning network configuration?
Amazingly though, a Google search showed this is a known issue. I really like the title of this bug: [Manually Configuring Network Causes Massive, Unreversable, Failure](https://bugs.launchpad.net/ubuntu/+source/network-manager/+bug/185209).
I believe this will be the first of many rants this blog will see, so readers (yes all 1 of you... thanks honey), check back soon. I'll try and keep it G rated, but no guarantees :-)

