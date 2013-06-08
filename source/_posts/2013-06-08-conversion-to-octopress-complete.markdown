---
layout: post
title: "Conversion to Octopress complete"
date: 2013-06-08 12:27
comments: true
categories:
- aws
- blog
- static website
- s3
- tech
---
Well, I was up until the wee house of the night last night, but I finally converted my blog to [Octopress](http://octopress.org/) and have it hosted on AWS S3. I must say, I really like Octopress and am becoming a big fan of generated static websites (such as [Github pages](http://pages.github.com/)), where anything complicated is handled by JavaScript. The only unfortunate thing is that I couldn't bring the comments over from Wordpress and put them into Disqus.

Generating the site and pushing it to S3 is as easy as:

``` bash
rake generate
cd public
s3cmd sync . s3://blog.pas.net.au/
```

The source in case you are interested is on Github at:

https://github.com/pas256/blog

Thanks to [Jeff Barr](https://twitter.com/jeffbarr) for introducting me to Octopress on his [AWS Road Trip](http://awsroadtrip.com/).

Looking forward to getting back into this - it has been too long without a good rant. :)

