---
layout: post
status: publish
title: CF8 JRun init script error

date: '2008-07-21 11:55:00 +0100'
date_gmt: '2008-07-21 11:55:00 +0100'
tags:
- coldfusion
- jrun
---
Setting up a staging server environment which will run 3 instances of CF and looking to alter the init scripts so it fires them all up on boot. Took a look at the one the installer creates and thought simple enough to just put multiple calls in for each server instance. Oh no.
On our RHES4 platform we have CF starting up as the apache user, but, running the script causes a "This account is currently not available." error to be thrown because it has no shell and it's locked. After looking at another server running a server install I notice the issue is down to the su command issued.
JRun
```
su $RUNTIME_USER -c "$CF_DIR/bin/jrun -stop cfusion"
su $RUNTIME_USER -c "$CF_DIR/bin/jrun -start cfusion >&amp; $CF_DIR/logs/cfusion-event.log &amp;"
```
Server Install
```
su -s /bin/sh $RUNTIME_USER -c "export PATH=$PATH:$CF_DIR/runtime/bin;
export LD_LIBRARY_PATH=$LD_LIBRARY_PATH;
cd $CF_DIR/runtime/bin; nohup $CF_DIR/runtime/bin/cfmx7 -jar jrun.jar -autorestart -start coldfusion &amp;"
```
Key difference is the "-s /bin/sh" part. Altering my script to the following makes it work. Yay. Not sure if this a known bug of something funky in our setup, but, as it's a vanilla OS install I don't see what it could be.
```
su -s /bin/sh $RUNTIME_USER -c "$CF_DIR/bin/jrun -stop cfusion"
su -s /bin/sh $RUNTIME_USER -c "$CF_DIR/bin/jrun -start cfusion >&amp; $CF_DIR/logs/cfusion-event.log &amp;"
```
