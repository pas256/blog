---
layout: post
title: "Dependency Nightmare for Tomcat on Debian"
date: 2009-08-18 09:34
comments: true
categories:
- debian
- dependencies
- linux
- rant
- tech
- tomcat
- ubuntu
---
I would love to not have to install the real Java and Tomcat manually on Debian, but I have little choice in the matter. Take a look at this:

```
$ apt-get install tomcat5.5
Reading package lists... Done
Building dependency tree
Reading state information... Done
The following extra packages will be installed:
ant ant-gcj ant-optional ant-optional-gcj antlr build-essential debhelper
default-jdk default-jre default-jre-headless defoma dpkg-dev ecj ecj-gcj fastjar
file fontconfig fontconfig-config g++ g++-4.3 gappletviewer-4.3 gcj-4.3
gcj-4.3-base gettext gettext-base gij-4.3 gjdoc hicolor-icon-theme html2text
intltool-debian java-common java-gcj-compat java-gcj-compat-dev
java-gcj-compat-headless jsvc libantlr-java libantlr-java-gcj libasound2
libatk1.0-0 libatk1.0-data libbcel-java libcairo2 libcommons-beanutils-java
libcommons-collections-java libcommons-collections3-java libcommons-daemon-java
libcommons-dbcp-java libcommons-digester-java libcommons-el-java
libcommons-launcher-java libcommons-logging-java libcommons-modeler-java
libcommons-pool-java libcompress-raw-zlib-perl libcompress-zlib-perl libcups2
libdatrie0 libdb4.5 libdigest-hmac-perl libdigest-sha1-perl libdirectfb-1.0-0
libecj-java libecj-java-gcj libexpat1 libfile-remove-perl libfontconfig1
libfontenc1 libfreetype6 libgcj-bc libgcj-common libgcj9-0 libgcj9-0-awt
libgcj9-dev libgcj9-jar libgcj9-src libglib2.0-0 libglib2.0-data libgtk2.0-0
libgtk2.0-bin libgtk2.0-common libice6 libio-compress-base-perl
libio-compress-zlib-perl libio-stringy-perl libjaxp1.3-java libjaxp1.3-java-gcj
libjpeg62 liblog4j1.2-java liblog4j1.2-java-gcj libmagic1 libmail-box-perl
libmail-sendmail-perl libmailtools-perl libmime-types-perl libmx4j-java
libobject-realize-later-perl libpango1.0-0 libpango1.0-common libpixman-1-0
libpng12-0 libregexp-java libservlet2.3-java libservlet2.4-java libsm6 libsqlite3-0
libstdc++6-4.3-dev libsys-hostname-long-perl libthai-data libthai0 libtiff4
libtimedate-perl libtomcat5.5-java libts-0.0-0 liburi-perl libuser-identity-perl
libxcb-render-util0 libxcb-render0 libxcomposite1 libxcursor1 libxdamage1
libxerces2-java libxerces2-java-gcj libxfixes3 libxfont1 libxft2 libxi6
libxinerama1 libxrandr2 libxrender1 libxtst6 make mime-support patch po-debconf
python python-central python-minimal python2.5 python2.5-minimal ttf-dejavu
ttf-dejavu-core ttf-dejavu-extra x-ttcidfont-conf xfonts-encodings xfonts-utils
...
0 upgraded, 146 newly installed, 0 to remove and 0 not upgraded.
Need to get 101MB of archives.
After this operation, 288MB of additional disk space will be used.
Do you want to continue [Y/n]?
```

WTF? I understand that Tomcat needs some kind of Java, but this is ridiculous. It is installing ant, fonts, compilers and worst of all, the most evil Java ever.

Ubuntu has the sense to make Sun Java available, but even if you do have Sun Java installed, the above is true on Ubuntu.

For shame!

I'll stick to downloading from [java.sun.com](http://java.sun.com/) and [tomcat.apache.org](http://tomcat.apache.org/).