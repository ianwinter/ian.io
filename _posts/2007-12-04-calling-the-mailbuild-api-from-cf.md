---
layout: post
status: publish
title: Calling the Mailbuild API from CF

date: '2007-12-04 15:42:00 +0000'
date_gmt: '2007-12-04 15:42:00 +0000'
tags:
- coldfusion
- mailbuild
- api
---
Had to help with someone out today who was trying to call the Mailbuild API from ColdFusion. It's a webservice and was proving a little annoying to get going, but after a bit of playing the final code looks like this.
```
<cfscript>
 params = structNew();
 params.ApiKey = '{yourkey}';
 params.ListID = '{yourlistid}';
 params.Email = 'name@domain.com';
 params.Name = 'Name';
</cfscript>
```
```
<cfinvoke webservice="http://{company}.createsend.com/api/api.asmx?wsdl"
      method="AddSubscriber"
      parameters = "#params#">
```
This is adding a new subscriber to a given list. Hopefully this might help someone integrating with CF!
