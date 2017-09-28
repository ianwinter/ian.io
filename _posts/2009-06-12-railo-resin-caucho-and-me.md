---
layout: post
status: publish
title: Railo, Resin, Caucho and me


date: '2009-06-12 23:06:40 +0100'
date_gmt: '2009-06-12 22:06:40 +0100'
tags:
- apache
- railo
- resin
- caucho
- centos
---
I've finally got resin, railo and caucho playing together in a nice, no longer ripping each other's hair, out kind of way. I have to say my biggest comment (and probably one of the more tricky things to do) is that for Railo to become a big player in trying to take away Adobe's CF user base installers are going to be essential. That said railo does seem quicker on processing CFML (need to do more tests to verify that).
I've got apache 2.2.3 running on CentOS 5.3. I have railo 3.1.0.015 (not updated to 016 yet) and caucho.
My resin.conf has this addition (multiple times for the different domains):
```
<host id="www.domain.co.uk" root-directory="/home/domain/public_html">
<host-name>www.domain.co.uk</host-name>
<host-alias>domain.co.uk</host-alias>
<web-app id="/" document-directory="."/>
</host>
```
The apache virtual hosts are just normal vhosts. No extras.
My caucho.conf (in /etc/httpd/conf.d) looks like this:
```
LoadModule caucho_module /usr/lib64/httpd/modules/mod_caucho.so
ResinConfigServer localhost 6800
CauchoConfigCacheDirectory /tmp
CauchoStatus yes
<Location /caucho-status>
SetHandler caucho-status
</Location>
```
Hopefully that might help someone out. I'm going to try and do a from scratch CentOS build guide for The Rackspace Cloud sometime next week. I've started migrating some sites over to the cloud server and so far not hit any issues.
