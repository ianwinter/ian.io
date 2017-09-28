---
layout: post
status: publish
title: ColdFusion Lockdown
date: '2013-05-20 11:56:13 +0100'
tags: coldfusion security web
---
So been going through various ColdFusion lock down tweeks, restrictions etc. and have a couple of useful links.
First up is the [Unofficial Updater](http://www.uu-2.info/). What it'll do is download the latest, relevant patches for your CF install and get them all in place. It saves a huge amount of time versus trying to do it yourself.

Second up is something off the back of the lock down guide relating to `GraphData.cfm`. It's not a real path, it's something that CF sorts out itself. If you choose to move CFIDE or restrict the path in the webserver this can cause issues. Either you leave it open, or, you can change the path it uses.
Credit to [Brandon](http://www.bpurcell.org/blog/index.cfm?entry=998&amp;mode=entry) for the following tweaks to move it about:

```
{cfmx-root}/lib/neo-graphing.xml
<strong>Below changes cfchart engine to generate the image path based on this config.</strong>
/CFIDE/GraphData.cfm ==> /images/GraphData.cfm
{cfmx-root}/wwwroot/WEB-INF/web.xml servlet mapping
<strong>When a request is handled to /images/Graphdata.cfm the GraphServlet will be
invoked to find and serve the charts.</strong>
/CFIDE/GraphData.cfm ==> /images/GraphData.cfm
```

*Note* It looks like the uu-2.info site isn't a thing anymore. Not sure if there's a replacement but as 9.0.2 is very old, I doubt it.
