---
layout: post
status: publish
title: Fixing Kerberos Issues With Apache Connector Rebuild

date: '2004-07-26 21:41:00 +0100'
date_gmt: '2004-07-26 21:41:00 +0100'
---
On the <a href="http://www.macromedia.com">Macromedia</a> site there is a TechNote titled
<a href="http://www.macromedia.com/support/jrun/ts/documents/apache_connector_rebuild.htm">Building the Apache Connector for JRun4 and ColdFusion MX</a>.
I've recently had to do this on a RedHat Enterprise 3 system with Apache 1.3 and had issues with it failing at the SSL/Kerberos point. This
is fixable and caused by a difference in the way RH ES3 has been layed out.
