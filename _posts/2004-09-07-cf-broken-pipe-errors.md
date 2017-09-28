---
layout: post
status: publish
title: CF Broken Pipe Errors

date: '2004-09-07 11:45:00 +0100'
date_gmt: '2004-09-07 11:45:00 +0100'
tags:
- coldfusion
---
On one of my sites we've been noticing a lot of broken pipe errors and connection reset messages. At first we couldn't find a problem, we've since worked out it's due to the cfcontent tag.
It seems that if the connection falls for any reason a broken pipe error is being given to the application.log / server.log
We're using the page for dynamic document serving by the way. The fix which seems to have solved the problem is to put an empty cfcatch round the cfcontent, now although this is a hack it stops the errors.
<div class="code"><cftry>
  <cfcontent ... >
  <cfcatch type="any"><i><!--- caught ---></i></cfcatch>
</cftry></div>
Not that good but it prevents the log file getting huge!
