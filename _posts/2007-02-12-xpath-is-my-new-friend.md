---
layout: post
status: publish
title: XPath is my new friend

date: '2007-02-12 22:29:00 +0000'
date_gmt: '2007-02-12 22:29:00 +0000'
tags:
- coldfusion
- xml
- xpath
- code
---
Doing something at work which involves me grabbing an XML feed from a third party provider on a daily basis then sticking it in a database so it can be reused as the feed only updates at midnight GMT.
Quite an easy task, cfhttp the file, loop over it, insert to the database. Well, this was made even easier by me discovering the entirely not new <a href="http://en.wikipedia.org/wiki/XPath">XPath</a> (or <a href="http://www.w3.org/TR/xpath">here</a>). Another quick google found a <a href="http://www.w3schools.com/xpath/default.asp">tutorial</a> and after a quick play using xmlParse() and xmlSearch() in Coldfusion 7 my feed was filtered and inserted for reuse. It's been around for a while, but, if you're using CF7 and working with XML and haven't seen XPath I suggest you take a look.
Quite nice as well as I actually learned something today!
