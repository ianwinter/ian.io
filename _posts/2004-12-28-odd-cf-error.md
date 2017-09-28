---
layout: post
status: publish
title: Odd CF Error

date: '2004-12-28 17:26:00 +0000'
date_gmt: '2004-12-28 17:26:00 +0000'
tags:
- coldfusion
---
Been scratching my head for a while trying to figure out why a simply CFC method with a couple of arguments and a plain select query was throwing an error. The error was
<strong>name can't be empty</strong>
no case, no real clue as to what it was. Turns out after some digging and googling (<a target="_blank" href="http://www.houseoffusion.com/lists.cfm/link=m:4:35865:180901">HOF - cf-talk</a> ) that it comes from if you have a query in the form
cfquery name="qname" dsn="#dsn#"
and your dsn variable is an empty string it throws that error. Just got to figure our were in my code the bug is!
