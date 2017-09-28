---
layout: post
status: publish
title: BBC News Pod

date: '2004-07-27 22:49:56 +0100'
date_gmt: '2004-07-27 22:49:56 +0100'
tags:
- coldfusion
- blogcfc
---
I'm in a pod mood. Here's another one, this time it'll get the latest 5 news headlines from the BBC News site.
<ol>
<li>Quite easy this one, first goto <a href="http://news.bbc.co.uk">BBC News</a> and find the feed you want to use. The RSS links are at the bottom of the page.</li>
<li>Create a file called bbc_news.cfm and put it in your {blogroot}/includes/pods that contains <a href="#" onClick="launchCodeView('/coldfusion/blogcfc_pods/bbc_news/bbc_news.txt');">this code</a>.</li>
<li>Update your {blogroot}/tags/layout.cfm to include the new pod.</li>
</ol>
Your done. Easy as pie.
Revision 1 - Code changed 29/07/04 21:12, DTD hack removed as BBC altered the feed.
