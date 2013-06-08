---
layout: post
title: "Rackspace Cloud API PHP Library"
date: 2009-08-13 21:42
comments: true
categories:
- api
- cloud
- open source
- php
- rackspace cloud
---
I am pleased to annouce my very first open source project hosted at github. The project, called [Rackspace Cloud PHP Library](http://github.com/pas256/Rackspace-Cloud-PHP-Library) is a simple, single PHP file to easily make [Cloud Server API](http://www.rackspacecloud.com/cloud_hosting_products/servers/api) calls. Rackspace have not yet released any libraries for their API, possibly because it is still kind of in beta. If they do, I believe there will be of little use for my project, but right now, it has value.

So what was my motivation for this?

Well the [Rackspace Cloud Management Console](https://manage.rackspacecloud.com/) is severely lacking in features. Things such as creating an image of a server (just like you can in AWS EC2), and sharing an IP address between servers (something you cannot do in EC2 - an IP address can only be attached to a single instance at a time). It is this second feature that I am most interested in because it means I can use a virtual IP address (floating IP) to create a HA (highly available) "cluster" of Tomcat servers. I plan on using [keepalived](http://www.keepalived.org/) to do the IP switching, and Apache with [mod_proxy_balancer](http://httpd.apache.org/docs/2.2/mod/mod_proxy_balancer.html) and [mod_proxy_ajp](http://httpd.apache.org/docs/2.2/mod/mod_proxy_ajp.html) to talk to multiple Tomcat servers. Without reading the [poorly written API documention](http://docs.rackspacecloud.com/servers/api/cs-devguide-latest.pdf) and learning that I could create a shared IP group, I would not have known this was possible.

This project is very much a work in progress, and what is in there now represents only about 8 hours of work. I welcome any feedback, and anyone else who wants to join.