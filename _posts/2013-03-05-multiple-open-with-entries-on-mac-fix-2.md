---
layout: post
status: publish
title: Multiple "open with" entries on Mac fix
date: '2013-03-05 10:14:21 +0000'
tags: mac macos tips-n-tricks
---
If you're running Mac OS you might have lots of duplicate entries in your "open with" menu. Thanks to a quick Google I found [this post](http://simplicitybliss.com/2012/11/multiple-open-with-entries-in-mac-os-x-finder) which details the fix, which, is nice and simple!

``` shell
/System/Library/Frameworks/CoreServices.framework/Frameworks/LaunchServices.framework/Support/lsregister -kill -r -domain local -domain system -domain user
```

You'll need to the relaunch /force quit finder or reboot. Only issue I had was a couple of default "open with" options had reset, but, get info on those types and reset takes no time at all.
