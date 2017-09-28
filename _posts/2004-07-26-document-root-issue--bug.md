---
layout: post
status: publish
title: Document Root Issue / Bug?

date: '2004-07-26 21:46:12 +0100'
date_gmt: '2004-07-26 21:46:12 +0100'
---
Found an issue that if you have a file called aaa.cfm (for example) placed in the ColdFusion MX default server webroot (/opt/coldfusionmx/wwwroot), have the default server disabled with cacherealpaths=false and have another web server (Apache or Sun ONE) configured to use it point at a document root (ie /www/site1) the file aaa.cfm will still be served up. Likewise if you place an index.cfm in the CFMX webroot it will override the file in you /www/site1 directory. Looking into if this needs to be raised as a bug.
