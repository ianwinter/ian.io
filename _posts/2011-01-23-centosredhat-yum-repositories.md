---
layout: post
status: publish
title: CentOS/Redhat YUM repositories


date: '2011-01-23 21:05:03 +0000'
date_gmt: '2011-01-23 21:05:03 +0000'
tags:
- centos
- yum
- redhat
- repositories
---
There's a bunch of stuff that you can't always find in the standard CentOS/Redhat repositories. The main&nbsp;repositories&nbsp;I've used are <a href="http://fedoraproject.org/wiki/EPEL/FAQ">EPEL</a>, <a href="http://iuscommunity.org/getting-started/">IUS</a>, <a href="http://blog.famillecollet.com/pages/Config-en">REMI</a> and in a specific case Cloudera's.&nbsp;I used the <a href="https://docs.cloudera.com/display/DOC/CDH3+Installation#CDH3Installation-InstallingCDH3onRedHatSystems ">Cloudera</a> one for flume, but, it's a quick way to get it installed and the various Hadoop odd's and end's.&nbsp;The IUS one is good for MySQL 5.1 and Percona builds, also Git 1.7. The EPEL repo has stuff like Redis and also other newer versions.
If you don't want to use one you can alias commands to use repos, so, for instance if you have a CentOS build with all the above install you'd get output from yum repolist something like this:
```
addons &nbsp; &nbsp; &nbsp; &nbsp;CentOS-5 - Addons
base &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;CentOS-5 - Base
cloudera-cdh3 Cloudera's Distribution for Hadoop, Version 3
epel &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;Extra Packages for Enterprise Linux 5 - i386
extras &nbsp; &nbsp; &nbsp; &nbsp;CentOS-5 - Extras
ius &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; IUS Community Packages for Enterprise Linux 5 - i38 enabled
updates &nbsp; &nbsp; &nbsp; CentOS-5 - Updates
```
If you want to install/search from a specific repo you can either disable them all in the /etc/yum.repos.d/*.repo files and then enable them when you run yum, or, you can leave them all enabled and disable them one by one. Something like this:
<div id="_mcePaste">```

alias yum-ius='yum --disablerepo=addons,base,epel,extras,updates,cloudera-cdh3'
alias yum-cloudera='yum --disablerepo=addons,base,epel,extras,updates,ius'
alias yum-epel='yum --disablerepo=addons,base,extras,updates,ius,cloudera-cdh3'

```
</div>
