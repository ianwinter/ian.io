---
layout: post
status: publish
title: A yummy replacement


date: '2011-09-14 23:14:59 +0100'
date_gmt: '2011-09-14 22:14:59 +0100'
tags:
- mysql
- centos
- yum
- php
---
Had a need to quickly (and easily) replace some yum packages this evening, namely getting mysql5.0 to 5.5 and php5.1 up to 5.3. I wasn't looking forward to doing this manually as keeping the dependencies in check is a faff at best. A quick Google however led me to the package yum-plugin-replace.&nbsp;This very easily sorts all the plugins out, flips them in out, shakes them all about and quicker than you can say Robert's your mother's brother sorts it all out.
Getting it going is easy:
```
yum install yum-plugin-replace
yum replace mysql --replace-with mysql55
```
Do the usual confirmations and you're done! Same goes with php to php53. This does of course assume you've already <a title="CentOS/Redhat YUM repositories" href="https://archive.ianwinter.co.uk/2011/01/23/centosredhat-yum-repositories/" target="_blank">installed</a> the <a href="http://fedoraproject.org/wiki/EPEL" target="_blank">EPEL</a> and <a href="http://iuscommunity.org/Repos" target="_blank">IUS</a> channels to get this packages from.
On a side note, if you do mysql don't forget to run the upgrade script.
```
mysql_upgrade --password
```
