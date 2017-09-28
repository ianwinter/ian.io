---
layout: post
status: publish
title: Submit for review
date: '2012-12-19 21:13:30 +0000'
tags: blogging wordpress mysql
---
If you run Wordpress and have updated it, in my case to 3.5, you may have come across the bug where the "Publish" button disappears and all you can do is "Submit for Review".

Now, a google search for this returns thousands of results, the fixes range from auto_increment values being full, harddrives being full, dodgy plugins, non-latest version plugins and DB corruption.

For me, non of those fixes worked. I started backing up the DB tables just by doing a simple duplication and on one table hit an error telling me that the default value (0000-00-00 00:00:00) wasn't valid for a datetime column. I'm running MariaDB, but, it's essentially MySQL - but fixed.
Turns out the fix is to remove `NO_ZERO_DATE,NO_ZERO_IN_DATE` from the sql_mode in the `my.cnf`. Restart and the issue goes away.

Interestingly on the previous Wordpress version I had a plugin installed (Incorrect Datetime Bug Fix) but had to uninstall it because it broke 3.5.
Wordpress need to get their date handling in order!
