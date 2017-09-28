---
layout: post
status: publish
title: apache2 issue on Leopard

date: '2007-11-10 19:21:00 +0000'
date_gmt: '2007-11-10 19:21:00 +0000'
tags:
- apple
- mac
- software
- apache2
---
So last geek post on a Saturday night before I go out.
Trying to get a site running which uses a lot of rewrite rules. Enable the module in the /etc/apache2/httpd.conf file, configtest and restart. That should be it, erm, no. All I saw when browsing the site was a nice 404 page. Checking in the /var/log/apache2/error_log I see the following line:
```
[Sat Nov 10 19:13:24 2007] [error] [client 127.0.0.1] Negotiation: discovered file(s) matching request: /Users/ianwinter/Sites/domainname/public_html/content (None could be negotiated)., referer: http://www.domainname.co.uk.local/
```
Now I already knew that apache2 handles the Order directive for allow & deny differently so I'd already changed the main directory block to the following to allow everything:
```
<Directory />
    Options FollowSymLinks
    AllowOverride All
    Order deny,allow
    Allow from all
</Directory>
```
Turns out after unsuccessful Google searches and me going back to trying all sorts (good old chmod -R 777 included) that unless you specifically setup a directory block for the /Users/ianwinter/Sites path it doesn't listen.
```
<Directory /Users/ianwinter/Sites/*>
    Options All
    AllowOverride All
    Order deny,allow
    Allow from all
</Directory>
```
It doesn't do this on Windows but I seem to recall having to do something on a RedHat system before. Maybe it's a *NIX thing.
