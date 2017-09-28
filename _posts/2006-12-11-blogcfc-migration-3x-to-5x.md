---
layout: post
status: publish
title: BlogCFC Migration 3.x to 5.x

date: '2006-12-11 21:21:00 +0000'
date_gmt: '2006-12-11 21:21:00 +0000'
tags:
- coldfusion
- blogcfc
---
If you're running and old version of BlogCFC and you are looking to upgrade one of the issues, well, nice to have things you'll have to do is create aliases for all the old entries and categories.
I've done a quick, very dirty, CF page that'll do this for you. This was run on 5.5.003, but, think the structure has been like it since 4.x, though, may be wrong. Anyway, might be useful.
<more />
``` cfm

    <cfset dsn = "blogdsn">
    <cfquery datasource="#dsn#" name="e">
    select * from tblblogentries
    </cfquery>
    <cfloop query="e">
       <cfquery datasource="#dsn#">
       update tblblogentries set alias = '#lCase(reReplace(reReplace(title,"[^A-Za-z0-9 ]","","ALL"),"( +)","-","ALL"))#' where id = '#id#'
       </cfquery>
    </cfloop>
    <cfquery datasource="#dsn#" name="c">
    select * from tblblogcategories
    </cfquery>
    <cfloop query="c">
       <cfquery datasource="#dsn#">
       update tblblogcategories set categoryalias = '#lCase(reReplace(reReplace(categoryname,"[^A-Za-z0-9 ]","","ALL"),"( +)","-","ALL"))#' where categoryid = '#categoryid#'
       </cfquery>
    </cfloop>

```
