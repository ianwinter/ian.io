---
layout: post
title: ColdFusion, isolation levels and MySQL 5.1
tags:
- coldfusion
- java
- mysql
- replication
status: publish
type: post
canonical_url: https://dev.venntro.com/2010/09/coldfusion-isolation-levels-and-mysql-5-1/
published: true
author: iwinter
meta:
  _edit_last: "24"
  _syntaxhighlighter_encoded: "1"
  _pingme: "1"
  _encloseme: "1"
  _wp_old_slug: ""
---
<p>We're always looking to make the platform run faster and better. One of the things that was on the list was to investigate upgrading all our MySQL servers from 5.0 to 5.1. This had a few reasons behind it some of those being better performance, bug fixes and enhanced features.</p>

<p>Our plan to roll out the upgrade (after extensive local and staging testing) was to upgrade all our slave databases 1 by 1 then eventually upgrade the master in a scheduled maintenance slot. This worked fine up until we got to the last one, the master database. The upgrade itself worked as expected and MySQL started up without error. Direct test SELECT/INSERT's statements were all fine however, after a few seconds later we started getting errors like this:</p>

{% highlight mysql %}
[Warning] Statement may not be safe to log in statement format. Statement: DELETE FROM .....
{% endhighlight %}

<p>What this meant was MySQL wasn't happy with the way it had received the statement and didn't want to replicate it. This had two consequences one of which was that the replication stopped and secondly, and more importantly, our application went down. We got the application back up within a couple of minutes by changing the replication format to <code>ROW</code> although this generated a lot more (GB's more) data in logging and was not sustainable.</p>

<p>After some more diagnosis, Google searches and tech note reading we narrowed it back to ColdFusion having something to do with the problem as all breaking requests were coming via our application servers. It turned out that ColdFusion forces all write queries to be sent with a transaction isolation level of <code>READ_UNCOMMITTED</code> which is incompatible with the replication log format (<code>STATEMENT</code>) that we are using. I'm not going into details on the replication format types but if you want to check out the <a href="http://dev.mysql.com/doc/refman/5.1/en/replication-formats.html">MySQL site</a>.</p>

<p>Turns out there is no nice way to stop ColdFusion doing this and highlights the fact CF doesn't do what it's supposed to, a bit like the leap year bug they still haven't fixed (but that's <a href="http://ianwinter.co.uk/2008/02/28/coldfusion-possible-leap-year-bug/">another</a> <a href="http://ianwinter.co.uk/2009/07/13/coldfusion-leap-year-bug-still-there/">post</a>).</p>

<p>The fix we deployed involves compiling MySQL Connector/J with a horrible hack to ignore any calls to change the transaction isolation level from the one initially provided by the server (which is set correctly). Depending on the version of ColdFusion you're using you'll need a different version of the connector. For ColdFusion 9, <a href="http://dev.mysql.com/get/Downloads/Connector-J/mysql-connector-java-5.1.13.tar.gz/from/http://mirrors.dedipower.com/www.mysql.com/">5.1.13</a> and for ColdFusion 8, <a href="http://dev.mysql.com/get/Downloads/Connector-J/mysql-connector-java-5.0.8.tar.gz/from/http://mirrors.dedipower.com/www.mysql.com/">5.0.8</a>. We didn't test this on any older versions of ColdFusion.</p>

<h3>Compiling with Java 1.5</h3>

<p>This version of Connector/J requires Java 1.5 to compile, which doesn't come with Snow Leopard, but is fairly easy to get going. Download a copy of the Java 5 package that was included in 10.5 (Leopard):</p>

{% highlight bash %}
$ cd /tmp/
$ curl -o java.1.5.0-leopard.tar.gz http://www.cs.washington.edu/homes/isdal/snow_leopard_workaround/java.1.5.0-leopard.tar.gz
$ tar zxvf java.1.5.0-leopard.tar.gz
$ sudo mv 1.5.0 /System/Library/Frameworks/JavaVM.framework/Versions/1.5.0-leopard
{% endhighlight %}

<p>Tell OS X that this is the Java 5 that you actually want to use:</p>

{% highlight bash %}
$ cd /System/Library/Frameworks/JavaVM.framework/Versions
$ sudo rm 1.5 1.5.0
$ sudo ln -s 1.5.0-leopard 1.5
$ sudo ln -s 1.5.0-leopard 1.5.0
{% endhighlight %}

<p>Then we can build the connector using the following instructions, there are two sets depending on your ColdFusion version.</p>

<h3>ColdFusion 9</h3>

{% highlight bash %}
$ cd /path/to/wld-library/mysql-connector-java/5.1.13
$ JAVA_HOME=/System/Library/Frameworks/JavaVM.Framework/Versions/1.5/Home ant dist
{% endhighlight %}

<p>The connector jar will be built in:</p>

{% highlight bash %}
build/mysql-connector-java-5.1.13-WLD/mysql-connector-java-5.1.13-CUSTOM-bin.jar
{% endhighlight %}

<h3>ColdFusion 8</h3>

{% highlight bash %}
$ cd /path/to/wld-library/mysql-connector-java/5.0.8
$ JAVA_HOME=/System/Library/Frameworks/JavaVM.Framework/Versions/1.5/Home ant dist
{% endhighlight %}

<p>The connector jar will be built in:</p>

{% highlight bash %}
build/mysql-connector-java-5.0.8-WLD/mysql-connector-java-5.0.8-CUSTOM-bin.jar
{% endhighlight %}

<h3>What's next?</h3>

<p>There's always something a bit better you can do. In our case I think we'll start to investigate the Percona builds of MySQL. They have a lot of plugins from Google/Facebook and other people providing extra stats and performance.</p>

<em>Orginally published at <a href="{{ page.canonical_url }}">dev.venntro.com</a></em>
