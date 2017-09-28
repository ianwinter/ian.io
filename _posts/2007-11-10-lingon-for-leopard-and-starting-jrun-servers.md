---
layout: post
status: publish
title: Lingon for Leopard and starting JRun servers

date: '2007-11-10 14:22:00 +0000'
date_gmt: '2007-11-10 14:22:00 +0000'
tags:
- coldfusion
- apple
- mac
- software
---
I still can't get CF8 to start correctly on Leopard so having used Lingon previously on Tiger I downloaded the latest version from <a href="http://lingon.sourceforge.net/">SourceForge</a>. It's been redesigned for Leopard as version 2.0.1 and looks really nice.
Configuration is easy, just add in a name, I choose com.adobe.jrunStartCfusion. In the "what" box just stick in "/Applications/JRun4/bin/jrun -start cfusion" and I only choose "Run it when it is loaded by the system (at startup or login)". Once you've got that in hit save, reboot (I think logout would work but hey) and you're done. Do a quick "ps -ef | grep cfusion" and you should see your CF process.
