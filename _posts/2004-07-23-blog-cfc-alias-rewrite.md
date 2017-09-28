---
layout: post
status: publish
title: Blog CFC Alias Rewrite

date: '2004-07-23 12:14:05 +0100'
date_gmt: '2004-07-23 12:14:05 +0100'
---
If you're using version 3.5 of the Blog CFC you can get Apache to create some nice URL to access posts which you've created aliases for.
Create a .htaccess file in you root directory and pop in it the following:
<div class="code">RewriteEngine on<br>
RewriteCond %{REQUEST_URI} ^/go/([0-9]*)<br>
RewriteRule ^(.*) /index.cfm?mode=alias&alias=%1  [L]</div>
You can <a href="/go/blogcfcaliasrewrite">try it</a> if you like.
