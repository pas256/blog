---
layout: post
title: "Backups by design on AWS EC2"
date: 2009-12-18 11:06
comments: true
categories:
- aws
- backup
- cloud
- howto
- scaling
- tech
---
My good mate\* Joel Spolsky wrote a [nice piece about backups (or rather, restoration)](http://www.joelonsoftware.com/items/2009/12/14.html), and I wanted to echo his remarks and how they relate to using AWS EC2.

If you are using EC2, you will quickly find that if an instance is terminated, any data on that instance is gone - lost forever. At first, this seems like a terrible idea, but in fact, it encourages you to get into best practices, and discover the awesome benefits of EBS.

We have many instances running of different types. We have built a "custom" Debian AMI for each of the instance types we use (web, database, management, etc). If you were to launch an instance with one of these AMIs, you would not have a fully working system. That is because these AMIs have sym-links for important and/or dynamic data. For example, on the web AMI we have created, `/etc/apache2`, `/etc/php5/` and `/var/www` are all sym-links. To where? A directory that an EBS volume is mounted to. That's right, all of the web configuration and website code only lives in an EBS volume. It is simple enough to write a little script that creates a nightly Snapshots of each EBS volume.

Now for the power of this setup. Every time you want to bring up another instance of the same type (say, for horizontally scaling), you are in fact doing a restoration from backup. Take a Snapshot (your backup), create an EBS volume, attach it to the new instance, and make it live! This doesn't just work for scaling, it works for bringing up staging servers that are mirrors of production or running experiments without affecting production.

We can even take it a step further! Those AMIs and Snapshots are all stored in S3 - data available to the whole Region. An instance and EBS volume exist in only 1 of the Availability Zones within that Region. You can use your backups to restore into a new Availability Zone which you can use to create a high-availability solution.

Happy scaling!

\* I don't know Joel personally - we have never met - but I do follow his work, like his company and LOVE [Fogbugz](http://www.fogcreek.com/FogBugz/)!