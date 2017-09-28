---
layout: post
status: publish
title: ColdFusion MX 7 Linux Install Errors

date: '2006-10-25 11:14:00 +0100'
date_gmt: '2006-10-25 11:14:00 +0100'
tags:
- coldfusion
- linux
---
Came across this the other day on a new server I was building.
<div class="code">$ ./coldfusion-702-lin.bin

Preparing to install...

Extracting the JRE from the installer archive...

Unpacking the JRE...

Extracting the installation resources from the installer archive...

Configuring the installer for this system's environment...



Launching installer...



./coldfusion-702-lin.bin: line 2317: /tmp/install.dir.31481/Linux/resource/jre/bin/java: Permission denied

./coldfusion-702-lin.bin: line 2317: /tmp/install.dir.31481/Linux/resource/jre/bin/java: Success


The workaround is set the IATEMPDIR variable
mkdir /opt/IATEMPDIR
export IATEMPDIR=/opt/IATEMPDIR</div>
<a href="http://livedocs.macromedia.com/coldfusion/7/htmldocs/wwhelp/wwhimpl/common/html/wwhelp.htm?context=ColdFusion_Documentation&file=00000005.htm" target="_blank">Taken from LiveDocs</a>
