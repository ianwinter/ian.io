---
layout: post
status: publish
title: MySQL 5.1 on CentOS with yum


date: '2010-08-30 17:43:13 +0100'
date_gmt: '2010-08-30 16:43:13 +0100'
tags:
- mysql
- centos
- yum
- remi
---
Not a particularly exciting first post back after so long with nothing but hopefully it's useful none the less. On a CentOS box you can't get MySQL 5.1 or PHP 5.3 easily from yum. Luckily a chap known as remi as a handy repo that contains both said packages.
To get the repo running you'll need to install a couple of RPM's:
<a href="https://archive.ianwinter.co.uk/wp-content/uploads/2010/08/epel-release-5-4.noarch.rpm">epel-release-5-4.noarch.rpm</a>
<a href="https://archive.ianwinter.co.uk/wp-content/uploads/2010/08/remi-release-5.rpm">remi-release-5.rpm</a>
To install them:
```
rpm -Uvh epel-release-5*.rpm remi-release-5*.rpm
```
After that jump into /etc/yum.repos.d where you should now have a remi.repo file. Edit that and change enabled=0 to enabled=1 and you're good to go. Do a yum check-updates and install away.
