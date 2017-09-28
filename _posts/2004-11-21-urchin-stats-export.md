---
layout: post
status: publish
title: Urchin Stats Export

date: '2004-11-21 19:52:00 +0000'
date_gmt: '2004-11-21 19:52:00 +0000'
tags:
- coldfusion
- urchin
---
Currently the host this site and a couple of other's are run on have the <a href="http://www.urchin.com">Urchin</a> web statistics package running. I have a need to expose these stats for a 3rd party.
Urchin do have a perl script that extracts data but, due to the server configuration it's not working correctly. I'm currently working on a small <abbr title="ColdFusion">CF</abbr> application will extract the stats via cfhttp and either display them on screen or write them to a specifed location. Should have the code available soon.
