---
layout: post
status: publish
title: Recompile Apache 1.3.x connector

date: '2006-10-31 19:21:00 +0000'
date_gmt: '2006-10-31 19:21:00 +0000'
tags:
- coldfusion
- linux
- adobe
- connector
---
If you're running Apache 1.3.x and Coldfusion 7 you might have had errors when starting apache about needing to compile it using DEAPI.
<blockquote><p>[warn] Loaded DSO /opt/coldfusionmx/runtime/lib/wsconfig/1/mod_jrun.so uses
plain Apache 1.3 API, this module might crash under EAPI!
(please recompile it with -DEAPI).</blockquote>
The original Adobe <a href="http://www.adobe.com/cfusion/knowledgebase/index.cfm?id=tn_18748">technote 18748</a> worked with CF6 as I've followed it before, but, with CF7 once compile and you try and start apache you get a crazy hex error, like this:
<blockquote><p>Cannot load /opt/coldfusionmx7/runtime/servers/lib/wsconfig/1/mod_jrun.so into
server: /opt/coldfusionmx7/runtime/servers/lib/wsconfig/1/mod_jrun.so: undefined symbol: hexstrtol</blockquote>
A quick google found <a href="http://www.van-sluis.nl/php/index.php?dest=coldfusion">this site</a> with a script on how to do it. Now one note on the guide is he's building a mod_jrun20.so file for apache 2, quick vi to change them all to mod_jrun.so and you're all good.
So, big thanks to that guy for posting his script. Mine is included below for reference.
```
#!/bin/bash
export CFMX=/opt/coldfusionmx7
export APACHE_PATH=/usr/local/apache
export APACHE_BIN=$APACHE_PATH/bin
#CFMX connector path eg $CFMX/runtime/lib/wsconfig/1
export CFMX_CONNECTOR=$CFMX/lib/wsconfig/1
#stop apache
$APACHE_BIN/apachectl stop
${APACHE_BIN}/apxs -c -Wc,-w -n jrun -S LIBEXECDIR=${CFMX_CONNECTOR}  mod_jrun.c
jrun_maptable_impl.c  jrun_property.c  jrun_session.c platform.c
jrun_utils.c jrun_mutex.c  jrun_proxy.c  jrun_ssl.c
${APACHE_BIN}/apxs -i -n jrun -S LIBEXECDIR=${CFMX_CONNECTOR} mod_jrun.la
strip   $CFMX_CONNECTOR/mod_jrun.so
```
Mine did error saying:
<blockquote><p>apxs:Error: file mod_jrun.la is not a DSO</blockquote>
but it all works.
