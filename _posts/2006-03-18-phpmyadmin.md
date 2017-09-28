---
layout: post
status: publish
title: phpMyAdmin

date: '2006-03-18 15:22:00 +0000'
date_gmt: '2006-03-18 15:22:00 +0000'
tags:
- mysql
---
Really simple comment for phpMyAdmin. Trying to install it on XP with MySQL 4.1 & Apache2. Setup the php.ini & httpd.conf file OK, php files rendered correctly all was going well. Set the path to my c:phpext dir and tried restarting but, kept getting errors:
<div class="code">"unable to load dynamic library '<a TARGET="_blank" HREF="c:php">c:php</a>extphp_mysql.dll'"</div>
Turns out that unless you copy libmysql.dll from the php root dir into c:windowssystem32 it won't work.
