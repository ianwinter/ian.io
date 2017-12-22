---
layout: post
title: Sphinx (part 1)
tags:
- data-storage
- indexing
- mysql
- search
- sphinx
status: publish
type: post
canonical_url: https://dev.venntro.com/2010/06/sphinx-part-1/
published: true
meta:
  _edit_last: "2"
  _pingme: "1"
  _encloseme: "1"
  _wp_old_slug: ""
canonical_url: https://dev.venntro.com/2010/06/sphinx-part-1/
---
<p>As part of our continued growth one of the main problems we came across was how to keep our searches running as quickly as possible so our members could find their perfect match easily. As the application grew some of the search queries running against MySQL were becoming complex and taking longer than we'd like to complete.</p>

<p>In the first instance a few simple solutions were put in place. All data that was required to be searchable was de-normalised into a single table with a few targeted indexes, a background task kept this data "flat" and up to date. This was taken a step further with those de-normalised tables being split be geographic region and network type based tables. This helped, but added an overhead to the process of updating the tables on a regular basis.</p>

<p>There are a few solutions out there that would work as search engines. MySQL queries, Solr and Sphinx - to name just three. We outgrew MySQL queries so had to look at the other alternatives. Solr is a fast, open source search platform based on the Apache Lucene project. Sphinx is a full-text search engine specifically designed to integrate well with SQL databases and scripting languages.</p>

<p>It took a week or so to run some feasibility tests to make sure it both did what we wanted it to and also that we could hook it up to our application which runs on ColdFusion. It became apparent quite quickly that Sphinx was going to be a lot faster to integrate with ColdFusion. A simple compilation of a jar file dropped in the right place, an invoked object and you're off!</p>

<p>At its most basic Sphinx has three parts: a source, the part that runs a sql statement and allocation of query columns to attributes, an index that specifics file storage location, minimum word length, matching rules etc. and lastly the searchd process that makes the index available and does the searching work. The index gets built and once the searchd process is up you can search. The indexes can be rebuilt whilst searches are still running by passing an --rotate option to the indexer which hup's the searchd process and makes your new index available right away.</p>

<p>When you have member data that changes constantly you don't want to rebuild the whole index all the time, it would take a while and is unnecessary. Luckily Sphinx allows the idea of a main + delta scheme or <a href="http://www.sphinxsearch.com/docs/current.html#live-updates">live updates</a>.</p>

<p>Basically you build your base index daily (for example) storing a timestamp of when that occurred then every 5 minutes build a delta index containing changes. When it comes to searching you specify and index, to search the base and delta you just tell Sphinx to search both and it'll sort it out for you. To get rid of anything you don't want, like members who have been removed as scammers, there is a handy <a href="http://www.sphinxsearch.com/docs/current.html#conf-sql-query-killlist">killlist</a> option which removes them and build time of the delta. There are other ways to search indexes in a distributed way but more on that next time.</p>

<p>For more information on Sphinx check out the <a href="http://www.sphinxsearch.com/">site</a>, <a href="http://www.sphinxsearch.com/docs/current.html">documentation</a> and <a href="http://www.sphinxsearch.com/wiki/doku.php?id=php_api_docs">wiki</a> for API examples.</p>

<em>Orginally published at <a href="{{ page.canonical_url }}">dev.venntro.com</a></em>
