---
layout: post
status: publish
title: Compilation Fun

date: '2008-03-30 21:53:00 +0100'
date_gmt: '2008-03-30 21:53:00 +0100'
tags:
- linux
- compilation
---
Building a new server today with a CLAMP installation. ColdFusion, Linux, Apache, MySQL and PHP.
Server in question was a RHES4 server. Going to install the following apps:
<ul>
<li>ColdFusion 7.0.2 (CU3)</li>
<li>apache 2.2.8</li>
<li>MySQL 5.0.51a</li>
<li>PHP 5.2.5</li>
</ul>
Suppose to be a quick job, but, oh no.
First up the server already had a MySQL 4.1 RPM installed which was causing some issues. When trying to do a straight update it failed.
```
rpm -Uvh MySQL-server-community-5.0.51a-0.rhel4
```
You can't remove it with rpm -e either. After some Google love discovered that it's some issues with compatibility. So off to the MySQL site again to wget the shared-compat RPM.
```
rpm -Uvh MySQL-shared-compat-community-5.0.51a-0.rhel4
```
Now the server will upgrade just fine. Next up get the client libraries and headers installed (we'll need them for the PHP compile later).
```
rpm -Uvh MySQL-devel-community-5.0.51a-0.rhel4 \
rpm -Uvh MySQL-client-community-5.0.51a-0.rhel4
```
I'll skip the crap about getting it running for now. Next up is apache.
Pretty easy compile this one, probably don't need rewrite and deflate separate but did it anyway.
```
./configure \
--prefix=/usr/local/apache2 \
--enable-mods-shared=most \
--enable-rewrite \
--enable-deflate
make
make install
```
Quick check that it runs and then onto PHP.
```
./configure \
--prefix=/usr/local/apache2/php \
--with-apxs2=/usr/local/apache2/bin/apxs \
--with-mysql=/usr \
--with-config-file-path=/usr/local/apache2/php \
--enable-force-cgi-redirect \
--disable-cgi \
--with-zlib \
--with-gettext \
--with-gdbm;
make clean;
make;
make install
```
The key stage is the make clean. For some odd reason if you don't make clean first you get an error like the following, the make clean will stop this happening.
```
httpd: Syntax error on line 55 of /usr/local/apache2/conf/httpd.conf: Cannot load /usr/local/apache2/modules/libphp5.so into server: /usr/local/apache2/modules/libphp5.so: undefined symbol: zend_ini_string
```
ColdFusion should be easy, will probably need to recompile the connector again but hey... that's the fun, I think.
