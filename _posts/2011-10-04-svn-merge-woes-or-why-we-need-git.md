---
layout: post
title: SVN merge woes, or why we need git
tags:
- git
- svn
status: publish
type: post
published: true
canonical_url: https://dev.venntro.com/2011/10/svn-merge-woes-or-why-we-need-git/
meta:
  _edit_last: "2"
  _syntaxhighlighter_encoded: "1"
  _pingme: "1"
  _encloseme: "1"
---
<p>Recently we've had some projects that were in branches, had been merged back on to the trunk and gone through our QA and testing processes. When it came to merging this project work into the deploy branch we discovered a few issues, odd merge conflicts, silly whitespace clashes and alike. All of these issues got resolved, the code was packaged up and pushed to the live servers. Done, or so we thought.</p>

<p>A few days later when it came to doing another live deploy, I got asked the question "why's SVN showing me a bunch of random files as being modified when I haven't touched them?". Initially I had no good answer, we validated and diff'd the files against the base copies and no changes were there. A quick Google for "files show as modified in svn when they're not" resulted in number of results all pointing towards issues with svn:mergeinfo properties.</p>

<p>The scenario that leads you in to this situation basically runs a bit like this (simplified!):</p>

<ul>
	<li>do some work on a branch
	<li>merge that branch to trunk
	<li>merge that change in to your deploy branch
	<li>resolve the conflicts in whatever way you need to
	<li>commit and deploy
</ul>

<p>Everything is committed, up to date, no files have changed and it's all working yet in your project working directory when you run a svn status you still have some miscreant files.</p>

{% highlight bash %}
[user@host ~]$ cd project
[user@host project]$ svn status
 M      path/to/file/1.html
 M      path/to/file/2.html
 M      path/to/file/3.html
{% endhighlight %}

<p>Remove the offending mergeinfo property flags.</p>

{% highlight bash %}
[user@host project]$ svn propdel svn:mergeinfo -R
property 'svn:mergeinfo' deleted from 'path/to/file/1.html'.
property 'svn:mergeinfo' deleted from 'path/to/file/2.html'.
property 'svn:mergeinfo' deleted from 'path/to/file/3.html'.
svn: Attempting to delete nonexistent property 'svn:mergeinfo'
[user@host project]$ svn revert .
Reverted '.'
{% endhighlight %}

<p>Commit the reverted, cleanup mergeinfo issues.</p>

{% highlight bash %}
[user@host project]$ svn ci -m "Removed mergeinfo issues."
Sending        path/to/file/1.html
Sending        path/to/file/2.html
Sending        path/to/file/3.html

Committed revision 1000.
{% endhighlight %}

<p>Make sure that your code is all sorted and updated.</p>

{% highlight bash %}
[user@host project]$ svn up
{% endhighlight %}

<p>Now, you can get back to what you wanted to do in the first place which was merge you changes.</p>

{% highlight bash %}
[user@host project]$ svn merge http://localhost/trunk -c 1001
--- Merging r1001 into '.':
U    path/to/file/2.html
{% endhighlight %}

<p>Now you can commit the changes.</p>

{% highlight bash %}
[user@host project]$ svn ci -m "Merging to deploy 1001"
Sending        .
Sending        path/to/file/2.html
Transmitting file data .
Committed revision 1002.
{% endhighlight %}

<p>Note that if you haven't got the code up to date you might see an error along theses lines:</p>

{% highlight bash %}
svn: Commit failed (details follow):
svn: File or directory '.' is out of date; try updating
svn: resource out of date; try updating
{% endhighlight %}

<p>You should now have a nice, tidy, up to date SVN repository.</p>

<h3>What next?</h3>

<p>All of these merge issues and the lengthy steps undertaken to get the house in order again raises the question what do we do next?</p>

<p>SVN was and is great when there are a handful of developers sitting next to each other all working on the same project. Everything is relatively simple and linear from a development / workflow standpoint. When that team grows as we have over the years this becomes more and more of a challenge. You need multiple branches for different projects, you have different teams working on them and eventually they all need to come back together.</p>

<p>So, what's the answer? In a word, git. All our new projects are now using it and we're slowly migrated the SVN repositories as well. There's many comparisons between SCM's on the web but with the rapid world of web app development git is shining through as the best choice. It allows for increased flexibility both in terms of it's functionality but also the ease multiple teams and projects can interact and be cherry picked to deploy when they're ready.</p>

<em>Orginally published at <a href="{{ page.canonical_url }}">dev.venntro.com</a></em>
