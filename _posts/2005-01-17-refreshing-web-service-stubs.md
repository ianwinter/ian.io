---
layout: post
status: publish
title: Refreshing Web Service Stubs

date: '2005-01-17 11:46:00 +0000'
date_gmt: '2005-01-17 11:46:00 +0000'
tags:
- coldfusion
---
If you have used CFMX and tried invoking webservices in any flavour, .NET, JAVA, CFML you'll have probably noticed that CF has an annoying (yet probably handy) habit of caching the stubs, so, if the webservice signature changes CFMX doesn't regonise those changes.
If it's a CF webservice you can use the administrator to delete the reference it creates and remove the stub, also a full server restart will clear the stubs. Neither of these are always possible (hosted environment for example) so you need to clear the stub's somehow else. Brandon Purcell has a very handy post titled <a href="http://www.bpurcell.org/blog/index.cfm?mode=entry&entry=965" target="_blank">Refreshing Web Service Stubs in ColdFusion MX</a>, which does exactly that.
