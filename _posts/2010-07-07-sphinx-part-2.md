---
layout: post
title: Sphinx (part 2)
tags:
- data-storage
- search
- sphinx
status: publish
type: post
canonical_url: https://dev.venntro.com/2010/07/sphinx-part-2/
published: true
author: iwinter
meta:
  _edit_last: "4"
  _pingme: "1"
  _encloseme: "1"
  _wp_old_slug: ""
  _syntaxhighlighter_encoded: "1"
---
<p>(This article is the second part in a series. Read <a href="/2010/06/sphinx-part-1/">Part 1</a>)</p>

<p>Having used Sphinx for a while, we found there was still room for improvement. Our new search engine worked but wasn't as quick as we would have liked under load. Initially the index was built along a one-index-for-all approach, containing every possible field that we may or may not have wanted to search on. Its primary use was to return searches to the main application, however there were also utility applications that needed search such as our newsletter tool.</p>

<p>The change we made to improve performance was fairly simple. We analysed which attributes we were actually querying against and removed all the attributes that were unused. This in itself would have probably been enough, however it still left quite a large index in terms of how many columns were in the query and led us to the next step of splitting out indexes. We tried using type based indexes, for example:</p>

{% highlight conf %}
src main_search
{
  ...
  sql_attr_uint = id
  sql_attr_uint = age
  sql_attr_uint = gender
  ...
}

src newsletter_search
{
  ...
  sql_attr_uint = id
  sql_attr_uint = age
  sql_attr_bool = emailpref
  ...
}

index main_search
{
  source = main_search
  path = /path/to/main_search
}

index newsletter_search
{
  source = main_search
  path = /path/to/main_search
}
{% endhighlight %}

<p>This new approach had a number of significant benefits:</p>

<ul>
	<li>The individual index size was reduced as unnecessary data was no longer stored</li>
	<li>The index rebuild time was reduced</li>
	<li>We gained the ability to take specific indexes offline for maintenance</li>
	<li>The end-to-end search time itself was reduced</li>
</ul>

<p>Next time: using distributed indexes with Sphinx.</p>

<em>Orginally published at <a href="{{ page.canonical_url }}">dev.venntro.com</a></em>
