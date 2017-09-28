---
layout: post
status: publish
title: CFMail and Gmail

date: '2008-07-11 12:20:00 +0100'
date_gmt: '2008-07-11 12:20:00 +0100'
tags:
- coldfusion
- gmail
- google
- code
---
Been looking a lot lately at various email related projects. One of the issues I noticed as part of this work is that if you send a mail with cfmail to a gmail account that only has a cfmailpart text/plain gmail won't render it.
If you send both, it works fine.
``` cfm

<cfmailpart type="text/plain">Plain</cfmailpart>
<cfmailpart type="text/html"><strong>Strong HTML</strong></cfmailpart>

```
So, if you're sending plain only, don't put it in a cfmailpart. Probably shouldn't have been in the first place, but, issue now solved.
