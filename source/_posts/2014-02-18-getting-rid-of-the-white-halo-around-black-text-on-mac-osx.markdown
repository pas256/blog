---
layout: post
title: "Getting rid of the white halo around black text on Mac OSX"
date: 2014-02-18 20:10
comments: true
categories:
- osx
- tips
- tech
- monitor
- mac
---
So I just purchased an external monitor for my home office. When I plugged it in to my Macbook Pro using a Mini-displayport to Displayport, the resolution was great (2560x1080), but it looked terrible. Everything was fuzzy, and the most annoying - black text had this white halo effect around it. I could still read everything, but it wasn't pretty.

After much searching, I found this post:

http://www.ireckon.net/2013/03/force-rgb-mode-in-mac-os-x-to-fix-the-picture-quality-of-an-external-monitor

It talks about the OSX thinking it was plugged into a TV, and using the YCbCr color space rather than RGB. After running the script, following the instructions and restarting, the monitor is working beautifully.

Hopefully this helps someone else, and will serve as a reminder to me should I encounter it again.
