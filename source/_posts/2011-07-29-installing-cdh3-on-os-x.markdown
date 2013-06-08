---
layout: post
title: "Installing CDH3 on OS X"
date: 2011-07-29 09:07
comments: true
categories:
- cdh3
- cloudera
- hadoop
- mac
- osx
- tutorial
---
Installing Cloudera’s version of Hadoop on an OSX Macbook Pro is not difficult, if you get the steps right.

Go to: https://ccp.cloudera.com/display/SUPPORT/CDH3+Downloadable+Tarballs
and download the Hadoop tarball.

We are going to run Hadoop in pseudo-distributed mode, which is nice in a dev environment.

Open up a Terminal window, and run:

``` bash
tar xvzf ~/Downloads/hadoop-0.20.2-cdh3u1.tar.gz
cd hadoop-0.20.2-cdh3u1/conf
cp ../example-confs/conf.pseudo/* .
```

Now we need to edit 2 files so that Hadoop knows where to write it’s data. This is when you decide where to write it. I did:

``` bash
mkdir -p ~/hadoop-data/cache/hadoop/dfs/name
```

Edit core-site.xml

``` xml
<property>
   <name>hadoop.tmp.dir</name>
   <value>/Users/${user.name}/hadoop-data/cache/${user.name}</value>
</property>
```

Next, edit hdfs-site.xml

``` xml
<property>
   <name>dfs.name.dir</name>
   <value>/Users/${user.name}/hadoop-data/cache/hadoop/dfs/name</value>
</property>
```

Finally, format HDFS, and start up the nodes:

``` bash
cd ../bin
./hadoop namenode -format
./start-all.sh
```

If you are typing in your password a lot, try this (assuming you have your SSH keys set up):

``` bash
cd ~/.ssh
cp id_rsa.pub authorized_keys
```

If you have upgraded to OS X Lion (v 10.7), then you might see this every time you do something:

```
2011-07-29 21:22:05.997 java[7690:1903] Unable to load realm info from SCDynamicStore
```

You can ignore it. It has something to do with Kerberos authentication (I think), but I don’t yet have a solution to getting rid of it.

