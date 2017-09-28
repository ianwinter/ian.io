---
layout: post
status: publish
title: sphinx, ColdFusion and geodist


date: '2009-06-30 23:16:26 +0100'
date_gmt: '2009-06-30 22:16:26 +0100'
tags:
- coldfusion
- api
- sphinx
- geodist
- geocoding
---
Over the past few days I've been looking into a new search engine for an application. The chosen search daemon is <a href="http://www.sphinxsearch.com/" target="_blank">sphinx</a> and it'll be called via the Java API (that comes with sphinx) from ColdFusion. Getting the basic searching going was pretty easy, creating an index, do a basic search, set some filters, done. The next step was to look into geodist matching, calcuating how far apart two latitude / longitude's are, slightly more tricky. In CF you can do it with some crazy curvature of the earth calculations but in sphinx it's real easy - when you know what it wants. The key there is radians.
You'll find plenty of tidbits on the web about this but no real end to end examples so hopefully this will help!
If you need to geocode some postcode or zip values I can highly recommend <a href="http://tinygeocoder.com/" target="_blank">tinygeocoder.com</a>. It's basic and gives you a straight lat / long value which is perfect. If you want something with a bit more information you'll need to check out the Yahoo! YDN <a href="http://developer.yahoo.com/maps/rest/V1/geocode.html" target="_blank">geocode</a> API or Google's HTTP API. I've got the YDN working with ColdFusion (I'll post about that later).
First up you'll need some data, I've created a basic DB table called <a href="https://archive.ianwinter.co.uk/wp-content/uploads/2009/06/sphinx_test.sql" target="_blank">sphinx_test.sql</a> which you can download. It's been written for MySQL. Quick note on the float values, if you don't specific float 10,6 (or something similar) your lat/longs will automatically be chopped to only 4 decimal places.
The SQL contains 3 locations in England. The British Airways London Eye (51.502893,-0.118811), Millennium Dome (51.501984,0.004764) and Windsor Castle (51.481971,-0.600686).
Once you've created your table you're ready to create an index. I'm going to assume you've already installed sphinx in /opt/sphinx (this is on a CentOS platform). To get the Java API part for ColdFusion is easy as well.
```
[root@hosting ~]# cd /root/sphinx-0.9.9-rc2/api/java
[root@hosting ~]# make
[root@hosting ~]# cp sphinxapi.jar /opt/coldfusion8/runtime/servers/lib
[root@hosting ~]# /opt/coldfusion8/bin/coldfusion restart
```
This is the <a href="https://archive.ianwinter.co.uk/wp-content/uploads/2009/06/sphinx.conf" target="_blank">sphinx.conf</a> you'll need to get this running. Now there's a whole load of stuff you could do in the configure file but I'll just give you a basic one which should be enough.
Note you have two options when it comes to getting the lat/long from MySQL, you can either use RADIANS(val) or use a pre-stored the radian value of the lat / long In this case we're going to use the latter but I've included a commented line for the former.
Now for the actual code that does the work. I've created a file called <a href="https://archive.ianwinter.co.uk/wp-content/uploads/2009/06/sphinx.cfm" target="_blank">sphinx.cfm</a> (view <a href="../wp-content/uploads/2009/06/sphinx.txt" target="_blank">as text</a> or click it to actually see it working) with comments inline. The interesting parts are:
```
<!--- sort order must be @geodist to allow distance matching --->
<cfset variables.sphinx.SetSortMode(variables.sphinx.SPH_SORT_EXTENDED, '@geodist desc')>
```
```
<!--- where are we starting, this will be the london eye latlong converted to radians --->
<cfset variables.sphinx.SetGeoAnchor('latitude', 'longitude', degreesToRadians(51.502893), degreesToRadians(-0.118811))>

```
All of the API calls are the same as the documented PHP functions so you can refer to the <a href="http://www.sphinxsearch.com/wiki/doku.php?id=php_api_docs" target="_blank">documentation</a> for more information.
Thanks go out to various bloggers and <a href="http://tim.bla.ir" target="_blank">Tim</a> to get this running.
