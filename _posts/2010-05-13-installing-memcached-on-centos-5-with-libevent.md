---
layout: post
status: publish
title: Installing memcached on CentOS 5 with libevent


date: '2010-05-13 22:03:18 +0100'
date_gmt: '2010-05-13 21:03:18 +0100'
---
There's quite a few guides out there to install memcached and libevent but here are the steps I took to get it installed. This is largely based around checking out my history entries, but, it doesn't the job!
Note that if you don't add the prefix to configure libevent you can end up with bits everywhere and it getting a little bit fiddly.
``` bash

[root@host ~] wget http://www.monkey.org/~provos/libevent-1.4.10-stable.tar.gz
[root@host ~] tar xvfz libevent-1.4.10-stable.tar.gz
[root@host ~] cd libevent-1.4.10-stable
[root@host libevent-1.4.10-stable] ./configure --prefix=/usr/local
[root@host libevent-1.4.10-stable] make &amp;&amp; make install
[root@host libevent-1.4.10-stable] cd ~
[root@host ~] vi /etc/ld.so.conf.d/libevent-i386.conf
	insert:
	/usr/local/lib/
[root@host ~] ln -s /usr/local/lib/libevent-1.4.so.2 /lib/libevent-1.4.so.2
[root@host ~] wget http://memcached.googlecode.com/files/memcached-1.4.4.tar.gz
[root@host ~] tar xvfz memcached-1.4.4.tar.gz
[root@host ~] cd memcached-1.4.4
[root@host memcached-1.4.4] ./configure --with-libevent=/usr/local
[root@host memcached-1.4.4] make &amp;&amp; make install

```
Now you should be able to run memcached.
``` bash
memcached -vv -u nobody
```
Alternatively of course you can use yum but that's no fun and not always the latest version.
