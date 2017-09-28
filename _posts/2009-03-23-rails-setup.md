---
layout: post
status: publish
title: Rails Setup


date: '2009-03-23 21:39:20 +0000'
date_gmt: '2009-03-23 21:39:20 +0000'
tags:
- apache
- mac
- rub
---
Been playing with Rails 2.3.2.1 and Passenger 2.1.2 but had some problems upgrading from the two respective earlier versions. The key was in the fact I probably didn't RTFM. Following the <a href="http://guides.rubyonrails.org">guides.rubyonrails.org</a> I'd got as far as section 4, changing the default index but whenever I did this or tried to view the controller directly on my Mac (10.5.6) it just 500 errored. The key was something that a friend (<a href="https://twitter.com/neilmiddleton" target="_blank">@neilmiddleton</a>) alluded to was that the app was running in production mode. There was no errors in the log files also aiding the diagnosis.
Solution? Pretty simple, just add an environment variable.
```
ServerName rortest.local
DocumentRoot "/Users/ian/Sites/rortest/public"
RailsEnv development
```
Now I've got to try and write something useful rather than hello world or a blog post/view page.
