---
layout: post
title: "Android Market links"
date: 2010-08-25 22:07
comments: true
categories:
- advice
- android
- market
- mobile
- tech
---
For those of you like me that have totally missed the [Publishing](http://developer.android.com/guide/publishing/publishing.html) page, here is how to create a link to your Android application in the Android Market:

    market://﻿details?id=<packagename>

OR

    http://market.android.com/details?id=<packagename>

So for [Remembory](http://www.gbott.com/apps/remembory), I have this:

    market://﻿details?id=com.gbott.remembory

The "old" way was to link to the search page by using this:

    market://search?q=pname:<package>

...but the details page method saves the user a click, or a tough decision when there are two applications both called Remembory.
