---
layout: post
status: publish
title: MAC apachectl ulimit error with 10.6.5


date: '2010-11-18 13:23:50 +0000'
date_gmt: '2010-11-18 13:23:50 +0000'
tags:
- mac
- apache2
- 10.6.5
---
If you've upgraded to the latest MAC release of Snow Leopard, 10.6.5, and you use apache you may have noticed a bug when you try to control apache with apachectl.
```

[ian@ian ~]$ sudo apachectl -t
/usr/sbin/apachectl: line 82: ulimit: open files: cannot modify limit: Invalid argument

```
The <a href="http://support.apple.com/kb/HT4435">release notes</a> detail that apache has been upgraded to 2.2.15 to fix some security holes, however by doing so causes this bug.
The fix is pretty easy and requires you to edit the /usr/sbin/apachectl file. The line you're looking for is this:
```
ULIMIT_MAX_FILES="ulimit -S -n `ulimit -H -n`"
```
All you need to do is simply remove the actual ulimit command and leave this in place:
```
ULIMIT_MAX_FILES=""
```
Try apachectl again and it'll work. In my case the offending line was in fact 64, not 82.
