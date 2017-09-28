---
layout: post
status: publish
title: RH9 MySQL 3.x to 4.0.x Upgrade

date: '2005-03-11 21:35:00 +0000'
date_gmt: '2005-03-11 21:35:00 +0000'
---
This might be of use, had MySQL 3.23.xx running on the RedHat 9 box which I needed to update to 4.0.24
This information I picked up off a the MySQL site but might be handy for someone.
----
I've successfully upgraded MySQL on a Redhat 9 server by following these steps:
1. Download the server, client, and "Dynamic client libraries
(including 3.23.x libraries)" rpms.
2. rpm -Uvh --nodeps MySQL-server-4.0.16-0.i386.rpm
3. rpm -Uvh MySQL-shared-compat-4.0.16-0.i386.rpm
4. rpm -Uvh MySQL-client-4.0.16-0.i386.rpm
5. I had to manually kill the mysqld process and restart, but after that everything works fine, including my php code.
----
You can download from <a href="http://dev.mysql.com/downloads/mysql/4.0.html">here</a> look for the RPM section.
