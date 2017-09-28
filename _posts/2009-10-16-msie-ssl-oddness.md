---
layout: post
status: publish
title: MSIE SSL Oddness


date: '2009-10-16 15:09:41 +0100'
date_gmt: '2009-10-16 14:09:41 +0100'
tags:
- apache
- ssl
- msie
- internet explorer
- ERROR_INTERNET_SECURITY_CHANNEL_ERROR
---
Internet Explorer strikes again, sometimes. In Chrome, Firefox and Safari the problem doesn't present. On a couple of webservers in a cluster we recently noticed the following type of error being returned randomly on images, CSS and JavaScript files when calling the page via SSL.
```
GET ERROR_INTERNET_SECURITY_CHANNEL_ERROR image/gif https://www.domain.com/path/to/image.gif

```
Now the majority of our SSL certificates for the site in question are server out via a SSL accelerator on our Cisco LBAL's but this site wasn't. It was still using a cert on the local box. Having dug deeper I noticed that a couple of the servers had the following lines of code in whereas all the others didn't. Due to the load balancing that solves the randomness side of it.
```
AddType application/x-x509-ca-cert .crt
AddType application/x-pkcs7-crl    .crl
SSLPassPhraseDialog  builtin
SSLSessionCache         shmcb:/var/cache/mod_ssl/scache(512000)
SSLSessionCacheTimeout  300
SSLMutex default
SSLRandomSeed startup file:/dev/urandom  256
SSLRandomSeed connect builtin
SSLCryptoDevice builtin
```
Something in those lines of code causes the issues, I'm stabbing in the dark that it's the session cache as none of the others would seem to be causing the problem. I've not tried line by line to find the offender.
Hopefully that will help someone out as I found lots of results in Google but not many solutions!
