---
layout: post
status: publish
title: Log response headers in apache


date: '2010-11-18 13:44:10 +0000'
date_gmt: '2010-11-18 13:44:10 +0000'
tags:
- apache
- apache2
- logging
- headers
---
I've been working on tweaking some apache logging on a few servers and one of the things I needed to log was a response header. The response header in question is an identifier with information about where the request was served from.
A quick look through the apache <a href="http://httpd.apache.org/docs/2.2/logs.html">log docs</a> doesn't give any clue on how to do this. I details request header logging but not response. A google search also didn't really come up with anything that useful until I stumbled on <a href="http://www.apacheweek.com/features/logfiles">an article</a> over on the &nbsp;apache week site.
A quick modification of the httpd.conf to duplicate the "common" log entry left me with this:
```
LogFormat "%h %l %u %t \"%r\" %>s %b <strong>\"%{HEADER_NAME}o\"</strong>" common2

```
The key element is the "o", if you have %{HEADER_NAME}i you'll get the request header, if you have&nbsp;%{HEADER_NAME}o you'll get the response header.
This results in the following entry in the log file:
```
127.0.0.1 - - [18/Nov/2010:13:37:39 +0000] "GET / HTTP/1.1" 200 31006 "<strong>HEADER_VALUE</strong>"

```
