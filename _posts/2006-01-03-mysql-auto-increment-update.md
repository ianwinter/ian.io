---
layout: post
status: publish
title: MySQL Auto Increment Update

date: '2006-01-03 21:27:00 +0000'
date_gmt: '2006-01-03 21:27:00 +0000'
---
Needed to do this and had to google around for it, so, here it is for ease (hopefully!)
You can reset an auto_increment field in mySQL by using the following syntax:<br>
<div class="code">alter table tablename auto_increment = #;</div>
Where # is an integer value.
