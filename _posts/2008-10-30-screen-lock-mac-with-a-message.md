---
layout: post
status: publish
title: Screen Lock Mac (with a message)

date: '2008-10-30 10:46:00 +0000'
date_gmt: '2008-10-30 10:46:00 +0000'
tags:
- apple
- mac
- software
---
There's a few ways you can screen lock a mac but I wanted to not just lock the screen also put a message in the login window.
Using <a href="http://www.handymacapps.com/">ScreenLock</a> from handymacapps.com and Quiksilver you can quickly launch the lock and then by running the following you can neatly add a message to the login window.
sudo defaults write /Library/Preferences/com.apple.loginwindow LoginwindowText "If you need me, call me."
Just do "" to blank it again or ditch the file.
Easy!
