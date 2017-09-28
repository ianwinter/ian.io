---
layout: post
status: publish
title: VirtualBox & CentOS


date: '2011-01-23 20:56:10 +0000'
date_gmt: '2011-01-23 20:56:10 +0000'
tags:
- centos
- virtualbox
- vm
---
If you've installed CentOS 5.x using Oracle (Sun) VirtualBox then you'll have probably had fun getting the guest additions to work. After doing this a few times, each time forgetting which commands I need I'm going to blog it if for nothing but refence!
```

sudo yum -y install gcc kernel-devel kernel-headers

```
After you've rebooted you should be good to go, again. If you don't have the right kernel it will moan about OpenGL support, but, you don't need it.
