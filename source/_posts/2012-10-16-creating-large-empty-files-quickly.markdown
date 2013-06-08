---
layout: post
title: "Creating large empty files quickly"
date: 2012-10-16 21:46
comments: true
categories:
- aws
- cloud
- linux
- tech
- ubuntu
---
I have seen many people on the interwebs using dd and /dev/zero to create empty files. This is great if the file is small, but for a 50Gb file, it simply takes too long, particularly on EC2. The solution? Truncate!

    truncate -s 50G my-large-file

Boom – instant.

This is great for doing things like mounting /tmp on EC2 to the ephemeral storage so /tmp is not limited to 10Gb (or whatever your root image size is)

    cd /mnt
    truncate -s 50G big-tmp
    mkfs.xfs /mnt/big-tmp
    mount -o loop /mnt/big-tmp /tmp
