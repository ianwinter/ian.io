---
layout: post
status: publish
title: Misleading PHP Error


date: '2010-09-05 20:53:33 +0100'
date_gmt: '2010-09-05 19:53:33 +0100'
tags:
- apache
- php
- short_open_tag
---
Having done some upgrades to my server (PHP, apache) I noticed I was getting some odd errors in the apache log and one of my PHP based sites wasn't loading. The error in the log was:
```
[Sun Sep 05 20:23:40 2010] [error] [client X.X.X.X] Request exceeded the limit of 10 internal redirects due to probable configuration error. Use 'LimitInternalRecursion' to increase the limit if necessary. Use 'LogLevel debug' to get a backtrace.
```
Now having done some digging everything suggested the rewrite rule was wrong, or, behaving differently after the upgrade but it turned out that the problem was in fact PHP short_open_tag related.
My php.ini had&nbsp;short_open_tag = Off (which it should, but the site in question was a quick one). Changing that to On and giving apache a restart made the problem go away. When I get a chance I will make the <? tags <?php but it's not a biggy right now.
For more on short_open_tag check <a href="http://www.php.net/manual/en/ini.core.php#ini.short-open-tag" target="_blank">the manual</a>.
<div></div>
