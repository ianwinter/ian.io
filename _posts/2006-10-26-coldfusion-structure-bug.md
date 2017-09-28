---
layout: post
status: publish
title: ColdFusion structure Bug

date: '2006-10-26 09:44:00 +0100'
date_gmt: '2006-10-26 09:44:00 +0100'
tags:
- coldfusion
- adobe
- bugs
---
Found a new bug today in Coldfusion 7. Basically if you create structure via arguments and try to use it later you'll notice some funky behaviour and the meta information about the structure is incorrect. Code test case is included.
<div class="code"><cfscript>
    function struct() { return arguments; } </cfscript>

<cfset s = struct(arg1="blahblah", arg2="cont", arg3="act")>

<cfoutput>
    
<h3>#s.getClass()#</h3>
    length: #structcount(s)# (#structkeylist(s)#)<br>
    <cfset structdelete(s, 'arg1')>
    length: #structcount(s)# (#structkeylist(s)#)<br>
    <cfset structdelete(s, 'arg2')>
    length: #structcount(s)# (#structkeylist(s)#) </cfoutput>

<cfset s = structnew()>
<cfset s['arg1'] = "blahblah">
<cfset s['arg2'] = "cont">
<cfset s['arg3'] = "act">

<cfoutput>
    
<h3>#s.getClass()#</h3>
    length: #structcount(s)# (#structkeylist(s)#)<br>
    <cfset structdelete(s, 'arg1')>
    length: #structcount(s)# (#structkeylist(s)#)<br>
    <cfset structdelete(s, 'arg2')>
    length: #structcount(s)# (#structkeylist(s)#) </cfoutput></div>
