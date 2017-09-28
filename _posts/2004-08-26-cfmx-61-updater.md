---
layout: post
status: publish
title: CFMX 6.1 Updater

date: '2004-08-26 09:48:00 +0100'
date_gmt: '2004-08-26 09:48:00 +0100'
tags:
- coldfusion
- adobe
- macromedia
---
The CFMX 6.1 updater has now been released, it contains a number of hotfixes previously released, security updates and more.
<a href="http://www.macromedia.com/support/coldfusion/ts/documents/cfmx61_updater.htm">Full Details</a>
<a href="http://www.macromedia.com/support/coldfusion/downloads_updates.html">Downloads Page</a>
Quick note on this, if you're installing on Red Hat Enterprise Server 3 (as I just have) you may the following error message, don't worry about it though it will installed fine.
<div class="code">root@server [/home/applications]# ./cfmx_61update_linux.bin
Preparing to install...
tail: `-1' option is obsolete; use `-n 1'
Try `tail --help' for more information.
./cfmx_61update_linux.bin: line 330: [: `)' expected, found -z
WARNING! The amount of /tmp disk space required and/or available
could not be determined. The installation will be attempted anyway.
Extracting the JRE from the installer archive...
Unpacking the JRE...
Extracting the installation resources from the installer archive...
Configuring the installer for this system's environment...

Launching installer...

Preparing CONSOLE Mode Installation...</div>
You're also going to need to recompile the connector if you're using Apache 1.3, doesn't seem to affect Apache 2.
