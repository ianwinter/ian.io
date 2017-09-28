---
layout: post
status: publish
title: Blog CFC This

date: '2004-09-27 14:13:00 +0100'
date_gmt: '2004-09-27 14:13:00 +0100'
tags:
- coldfusion
- blogcfc
---
I'm sure some of you will have seen <a href="http://www.blogger.com">Blogger's</a> <a href="http://help.blogger.com/bin/answer.py?answer=152&topic=17">BlogThis!</a> functionality, well, I decided to have a play and make a version for <a href="http://www.camdenfamily.com/morpheus/blog">Ray Camden's</a> Blog CFC.
After a little playing about I've finally got it working. 
Basically I took the editor.cfm and copied it, then added the login form to the actual page, removed some of the edit functionality as it'll only be used to add entries, alter the cfparam tags so that they have the URL variables in. Finally I copied the Blogger JavaScript code, simply changing the URL path to my new page. It now behaves exactly as the BlogThis! link does.
For full details on the code and how to install it read on...
