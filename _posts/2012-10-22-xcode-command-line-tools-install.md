---
layout: post
status: publish
title: Xcode Command Line Tools Install
date: '2012-10-22 13:24:28 +0100'
tags: mac macos xcode cli
---
If you've upgraded to Mountain Lion you might not have Xcode's command line tools installed anymore. Normally you can install via Xcode itself, but, if you're going to be doing this remotely you'll need command line instructions. It's actually not to tricky. You'll need a developer account for Apple though.

If you go to [https://developer.apple.com/downloads/index.action](https://developer.apple.com/downloads/index.action), login and then (at the time of writing) download "Command Line Tools (OS X Mountain Lion) for Xcode - October 2" you'll be on the way.

Next, mount the DMG:

``` shell
$ hdiutil attach -mountpoint /Volumes/xc xcode451cltools_10_86938200a.dmg
```

Then run the installer:

``` shell
$ sudo installer -pkg Command\ Line\ Tools\ \(Mountain\ Lion\).mpkg -target "/"
```

Lastly, detach:

``` shell
$ hdiutil detach /Volumes/xc
```
