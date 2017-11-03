---
layout: post
title: "Resolving SSL CA certificate errors with Typhoeus and cURL"
author: iwinter
tags:
  - curl
  - ssl
  - typhoeus
status: publish
type: post
published: true
original: https://dev.venntro.com/2016/01/issues-with-typhoeus-curl-and-ssl/
summary: >
  How to resolve SSL CA certificate errors with typhoeus, curl and SSL.
---

We've been working on a couple of projects recently, both of which require
API calls to endpoints which are available over HTTPS. The development had
been done on OSX (which has a recent version of libcurl) and tested on our
QA boxes which are running CentOS 6 (again, newer libcurl).

When the code was moved on to our staging environment we immediately hit an
error which indiciated an issue with SSL certificate validity. The HTTPS
requests from typhoeus were seemingly randomly failing with the error
`problem with the SSL CA`. This [typhoeus issue] had been reported before
and gave us something to go on.

## What Was Going Wrong?

Our development VM's, QA clouds and production are nearly all RedHat 6, but
due to a gap in our OS upgrade timeline staging was still on RedHat 5 and as
a result curl 7.15.5:

	$ curl --version
	curl 7.15.5 (x86_64-redhat-linux-gnu) libcurl/7.15.5 OpenSSL/0.9.8b zlib/1.2.3 libidn/0.6.5
	Protocols: tftp ftp telnet dict ldap http file https ftps
	Features: GSS-Negotiate IDN IPv6 Largefile NTLM SSL libz

As it happened both the ops team and one the development teams had been
working on resolving this issue, but, from two different angles. The
developer had been looking at how to give typhoeus the right SSL CA options,
whilst the ops team had been looking at the OS side of it. RedHat 6 does have
something to do with the issue, and between the two OS's there are a number
of differences between where and how various bits of OpenSSL, SSL CA's and
all kinds of SSL/TLS related items are stored.

## The Fix

After some, what can probably best be described as experiementation I ended up
with a quick fix that got got both teams to a working position without any
added development work needed.

I did some Googling and found the mirror site over at city-fan.org which had
the packages I was after.

* /sysutils/Mirroring/curl-7.41.0-1.0.cf.rhel5.x86_64.rpm
* /sysutils/Mirroring/libcurl7155-7.15.5-17.cf.rhel5.x86_64.rpm
* /sysutils/Mirroring/libcurl-7.41.0-1.0.cf.rhel5.x86_64.rpm
* /libraries/c-ares-1.10.0-4.0.cf.rhel5.x86_64.rpm
* /libraries/c-ares-devel-1.10.0-4.0.cf.rhel5.x86_64.rpm
* /libraries/libidn-1.30-2.rhel5.x86_64.rpm
* /libraries/libidn-devel-1.30-2.rhel5.x86_64.rpm
* /libraries/libmetalink-0.1.2-7.rhel5.x86_64.rpm
* /libraries/libmetalink-devel-0.1.2-7.rhel5.x86_64.rpm
* /libraries/libssh2-1.5.0-1.0.cf.rhel5.x86_64.rpm
* /libraries/libssh2-devel-1.5.0-1.0.cf.rhel5.x86_64.rpm

Once downloaded, I chose to add them to our own yum repo and the use yum to
upgrade what was there.

	yum erase curl-devel
	yum install libidn libssh2 libmetalink curl libcurl libcurl-devel libcurl7155

After that was all installed, a `curl --version` shows 7.41.0 and both teams got back on track.

<em>Orginally published at <a href="{{ page.original }}">dev.venntro.com</a></em>

[typhoeus issue]: https://github.com/typhoeus/typhoeus/issues/90
[mirror-sysutils]: http://mirror.city-fan.org/ftp/contrib/sysutils/Mirroring/
[mirror-libraries]: http://mirror.city-fan.org/ftp/contrib/libraries/
