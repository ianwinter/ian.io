---
layout: post
status: publish
title: BlogCFC & Blackstone

date: '2004-09-23 20:23:00 +0100'
date_gmt: '2004-09-23 20:23:00 +0100'
tags:
- coldfusion
- blogcfc
---
Ray posted about this on <a href="http://www.camdenfamily.com/morpheus/blog/index.cfm?mode=entry&entry=E821606F-CEE1-10FD-DA96A018EE722B88">his blog</a> but just hit this problem myself so thought I'd post up the revised code block.
<div class="code"><cfif not server.coldfusion.productversion gte 6.1>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<cfset throw("Blog must be run under ColdFusion MX 6.1 or higher.")>
&nbsp;&nbsp;&nbsp;</cfif></div>
