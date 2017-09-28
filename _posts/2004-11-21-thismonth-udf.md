---
layout: post
status: publish
title: thisMonth UDF

date: '2004-11-21 19:55:00 +0000'
date_gmt: '2004-11-21 19:55:00 +0000'
tags:
- coldfusion
---
Just submitted to <a href="http://www.cflib.org">CFLib.org</a> a new <abbr title="User Defined Function">UDF</abbr> that I've written called thisMonth().
thisMonth() will return a structure of structures containing information about the month including date object for each day, each day as a string and each day's date letters ie "st,nd,rd,th". Also start and end day as entries, number of days in that month and the month number.
It defaults to the current month but you can also pass a date object into it.
You can have a look at the <a href="/coldfusion/code/thisMonthUDF.cfm">example</a> or take a look at <a href="##" onclick="launchCodeView('/coldfusion/code/thisMonthUDF.cfm');">view the code</a>.
