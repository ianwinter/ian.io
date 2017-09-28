---
layout: post
status: publish
title: Text Area Counter

date: '2004-07-30 16:46:45 +0100'
date_gmt: '2004-07-30 16:46:45 +0100'
tags:
- coldfusion
---
Something that I've seen on other sites, particularly online SMS sending tools, is form textarea's that have a little box telling you how many characters you've typed, or that you have left to enter.
It's not that tricky to setup really. You need a form with a textarea, and a very simple Javascript function to count the code.
<div class="code">/* javascript function to update form field
 *  field&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;form field that is being counted
 *  count&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;form field that will show characters left
 *  maxchars &nbsp;&nbsp;&nbsp;maximum number of characters&nbsp;&nbsp;&nbsp;
*/
function characterCount(field, count, maxchars) {
&nbsp;&nbsp;&nbsp;if (field.value.length > maxchars) {
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;field.value = field.value.substring(0, maxchars);
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;alert("Erro<a TARGET="_blank" HREF="r:
">r:
</a>
- You are only allowed to enter up to "+maxchars+" characters.");
&nbsp;&nbsp;&nbsp;} else {
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;count.value = maxchars - field.value.length;
&nbsp;&nbsp;&nbsp;}
}</div>
The textarea needs a couple of event handlers so it knows to do something:
<div class="code"><textarea name="notes" cols="40" rows="5" wrap="virtual" onKeyDown="characterCount(this.form.notes,this.form.remaining,255);" 
onKeyUp="characterCount(this.form.notes,this.form.remaining,255);">
</textarea></div>
The full code can be <a href="##" onclick="launchCodeView('/coldfusion/code/textarea_count.cfm');">seen</a> or you can <a href="/coldfusion/code/textarea_count.cfm">view the demo</a>.
