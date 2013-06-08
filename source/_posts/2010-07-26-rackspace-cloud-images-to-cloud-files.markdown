---
layout: post
title: "Rackspace Cloud images to Cloud Files"
date: 2010-07-26 22:07
comments: true
categories:
- backup
- rackspace cloud
- tech
---
For doing anything serious on Rackspace Cloud, you need to be able to use machines larger than 2Gb of RAM. Problem is, machines larger than 2Gb of RAM cannot be imaged (or backed up) - until now. About a month ago, they announced the ability to [snapshot a machine into Cloud Files](http://www.rackspacecloud.com/blog/2010/06/16/introducing-cloud-servers-snapshots-to-cloud-files/). Today I decided to take it for a spin and hit Snag number one: there was no way to do an image on my new 16Gb machine. After talking talking to one person at Rackspace Cloud, I was none the wiser - Snag two was the lack of training for their support staff. After asking for the supervisor, he created a ticket for the approval for the terms (letting me know I would be charged for the storage in Cloud Files) and off I went. Until I hit Snag 3 - the "Images" tab for the server details still showed no way of performing an image. I was told to use the "My Server Images" and HORAY I could make an image of my 16Gb machine.

Finally the feature that prevented my company from using Rackspace Cloud, and instead using AWS, was fixed! Congratulations to the Rackspace Cloud team.

Another thing I learnt today is that there are two data centers for Rackspace Cloud machines - DFW and ORD. The first server you provision gets assigned to one of the two data centers, and which ever one it gets put into is the data center that all of your other servers will be put into as well. So if the first server gets put into DFW, then all of the other ones you create will be. That is until you delete all of your servers. Then once again, the first server can get put into any data center.

I am hoping that in the future, we will have a choice about which data center a server is provisioned in, particularly since it will help for geographical distribution.