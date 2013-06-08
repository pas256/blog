---
layout: post
title: "Git and SVN working together"
date: 2011-01-12 22:07
comments: true
categories:
- git
- svn
- tech
- tutorial
---
I have been using Subversion for a long time, and am relatively new to Git. This post is a little tutorial of what I have learnt getting Git and SVN to play nicely together, primarily using git-svn.

My goal is to maintain the code in the original SVN repository while transitioning the team to Git. This means changes to either repository get reflected into the other one.

First steps is to take the create an empty Git repository to import SVN into (this is your remote Git repository):

``` bash
cd $HOME/git-repo
mkdir project
cd project
git --bare init
```

Now clone the empty Git repository so you have a working directory, and the import the SVN repository into Git. The directory names need to match (in this case, they are both "project”):

``` bash
cd $HOME
git clone file://$HOME/git-repo/project
git svn clone -s file://$HOME/svn-repo/project
```

If all is going well, you should have a "project” directory with all of your files imported from SVN in it. This directory is also your Git clone. Now you can "push” these changes to your remote Git repository

``` bash
cd $HOME/project
git push origin master
```

Now make some changes to your working directory

``` bash
echo "This is a change in Git" > git-change.txt
git add git-change.txt
git commit -m "Adding a new file"
git push
```

We now have a change in Git that is not in SVN. To copy the change over to SVN we do a "dcommit”

``` bash
git svn dcommit
```

Lets check out the SVN repository, and make a change in there

``` bash
cd $HOME
svn checkout file://$HOME/svn-repo/project/trunk svn-project
cd svn-project
echo "Here is a change in SVN" > svn-change.txt
svn add svn-change.txt
svn commit -m "Adding a new change in SVN"
```

To get this change in Git, we need to "rebase”. This is not as scary as it sounds

``` bash
cd $HOME/project
git pull
git svn rebase
git push
```

Horay! We now know how to make changes go both ways between SVN and Git.