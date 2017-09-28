---
layout: post
status: publish
title: NT4 & CFMX 6.1 Updater

date: '2005-08-23 13:57:00 +0100'
date_gmt: '2005-08-23 13:57:00 +0100'
tags:
- coldfusion
---
Had an issue where a Windows NT4 server needed to have CF MX 6.1 installed on it. That all went fine, problem came when the 6.1 updater was attempted to be installed. The issue presented itself by the updater installer seemingly hanging on the 3rd screen, just before you click OK to preceed.
The issue is with the OS (NT4) not supporting DirectDraw (per Tech Support needs DirectX v8.1 or greater). You can get the updater to work though by specifying an environment variable to tell Java to not use 3d drawing and only use 2d.
You can set this value by right-clicking on "My computer", then:
Properties -> Advanced -> Environment Variables
Add a "New..." variable in "System variables" section, Variable Name is "_JAVA_OPTIONS" and the Variable Value set to "-Dsun.java2d.noddraw=true"
Hit apply and try running the installer again.
