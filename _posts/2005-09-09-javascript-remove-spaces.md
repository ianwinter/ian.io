---
layout: post
status: publish
title: Javascript remove spaces

date: '2005-09-09 09:14:00 +0100'
date_gmt: '2005-09-09 09:14:00 +0100'
---
It's a trivial thing which is quite often needed in Javascript, a quick easy way to remove spaces. Regex is the answer.
Sure you've seen something similar before but useful for reference non the less. Can be used against any string but this example is used on a form input box.
<div class="code"><FONT COLOR=NAVY><FONT COLOR=FF8000><input type=<FONT COLOR=BLUE>"text"</FONT> name<FONT COLOR=BLUE>"bob"</FONT> onBlur=<FONT COLOR=BLUE>"this.value = this.value.replace(/s*/g,');"</FONT>></FONT></FONT></div>
