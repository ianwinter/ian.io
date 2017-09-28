---
layout: post
status: publish
title: Coldfusion possible Leap Year bug

date: '2008-02-28 10:11:00 +0000'
date_gmt: '2008-02-28 10:11:00 +0000'
tags:
- coldfusion
- adobe
- bugs
---
My colleague noticed that as of midnight GMT one of our applications at work broke. After some digging it looks like a CF leap year bug.
``` cfm

<cfset attributes.minage = 18>
<cfoutput>
#now()#
#attributes.minage#
#dateadd("yyyy", -(attributes.minage), now())#
#datediff("yyyy", dateadd("yyyy", -(attributes.minage), now()), now())#
#datediff("yyyy", dateadd("yyyy", -(attributes.minage), now()), DateAdd("d", 1, now()))#
</cfoutput>

```
The code was run on CF 7.0.1, 7.0.2 &#038; 8.0.0 on Windows and Linux, all produce same results.
Not sure for definite it's a bug, but, it's always worked until today, in theory it might work again tomorrow as well. Last leap year of course was CF6 so may not have shown up.
Yay for dateDiff()... :|
<em>Update</em>
A more detailed example. It only effects the 28/2. Other dates fine suggesting more that it's leap year related.
``` cfm

<cfoutput>
	<cfset variables.datea = createDate(2008,02,3)>
	<cfloop from="2008" to="1990" step="-1" index="variables.y">
		<cfset variables.dateb = createDate(variables.y,02,3)>
		<cfset variables.diff = datediff("yyyy", variables.datea, variables.dateb)>
		#variables.datea# - #variables.dateb# = #variables.diff#
	</cfloop>
</cfoutput>

```
And it's output....
```

{ts '2008-02-28 00:00:00'} - {ts '2008-02-28 00:00:00'} = 0
{ts '2008-02-28 00:00:00'} - {ts '2007-02-28 00:00:00'} = 0
{ts '2008-02-28 00:00:00'} - {ts '2006-02-28 00:00:00'} = -1
{ts '2008-02-28 00:00:00'} - {ts '2005-02-28 00:00:00'} = -2
{ts '2008-02-28 00:00:00'} - {ts '2004-02-28 00:00:00'} = -4
{ts '2008-02-28 00:00:00'} - {ts '2003-02-28 00:00:00'} = -4
{ts '2008-02-28 00:00:00'} - {ts '2002-02-28 00:00:00'} = -5
{ts '2008-02-28 00:00:00'} - {ts '2001-02-28 00:00:00'} = -6
{ts '2008-02-28 00:00:00'} - {ts '2000-02-28 00:00:00'} = -8
{ts '2008-02-28 00:00:00'} - {ts '1999-02-28 00:00:00'} = -8
{ts '2008-02-28 00:00:00'} - {ts '1998-02-28 00:00:00'} = -9
{ts '2008-02-28 00:00:00'} - {ts '1997-02-28 00:00:00'} = -10
{ts '2008-02-28 00:00:00'} - {ts '1996-02-28 00:00:00'} = -12
{ts '2008-02-28 00:00:00'} - {ts '1995-02-28 00:00:00'} = -12
{ts '2008-02-28 00:00:00'} - {ts '1994-02-28 00:00:00'} = -13
{ts '2008-02-28 00:00:00'} - {ts '1993-02-28 00:00:00'} = -14
{ts '2008-02-28 00:00:00'} - {ts '1992-02-28 00:00:00'} = -16
{ts '2008-02-28 00:00:00'} - {ts '1991-02-28 00:00:00'} = -16
{ts '2008-02-28 00:00:00'} - {ts '1990-02-28 00:00:00'} = -17

```
