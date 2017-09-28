---
layout: post
status: publish
title: Comma's Are Evil

date: '2007-03-13 15:02:00 +0000'
date_gmt: '2007-03-13 15:02:00 +0000'
tags:
- code
---
Aside from the fact that CSS cross browser is often quite a fun exercise and we all know how lovely MSIE can be in it's quirks as indeed can Firefox, but, today I've leart that the comma is indeed evil.
The CSS file in question is quite large, a couple of hundred lines or so, in amungst it was a simple class:
``` cfm

#text p, { padding: 0; margin: 0; color: #ffffff; }

```
The issue was that the first paragraph tag in the text div had extra padding on the top whereas in IE it wasn't there. The comma should be an obvious spot but within the larger file I'd overlooked it. Ah well... back to the CSS grindstone!
