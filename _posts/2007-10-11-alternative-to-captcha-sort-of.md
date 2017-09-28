---
layout: post
status: publish
title: Alternative to Captcha, Sort Of

date: '2007-10-11 22:47:00 +0100'
date_gmt: '2007-10-11 22:47:00 +0100'
tags:
- coldfusion
- code
- captcha
---
I've seen a lot of Captcha (Completely Automated Public Turing Test to Tell Computers and Humans Apart) graphics on sites now-a-days, some of which are quite hard to read.
One alternative, and don't get me wrong it's not better because it's still text however it will make it a bit harder for bots to spam you, is to ask a question and have the user give the answer.
In this example I'll ask the question:
<blockquote><p>What is two added with four? Please enter the answer as a number.</blockquote>
Below is a very basic ColdFusion page with that example.
```
<!--- store the numbers as words in a list, could also been and array --->
<cfset numberWords = "one,two,three,four,five,six,seven,eight,nine,ten">
<!--- get two numbers --->
<cfset n1 = randrange(1,10)>
<cfset n2 = randrange(1,10)>
<!--- calculate the total --->
<cfset nt = n1 + n2>
<!--- hash that value so it can't be seen in the source --->
<cfset nh = hash(nt)>
<cfoutput>
What is #listGetAt(numberWords,n1)# added to #listGetAt(numberWords,n2)#?
</cfoutput>
<form name="frm" method="post" action="captcha.cfm">
	<!--- store the hash value in the form --->
	<input type="hidden" name="hashvalue" value="<cfoutput>#nh#</cfoutput>">
	<input type="text" name="answer">
	<input type="submit">
</form>
<!--- hash the user's answer and compare to the passed hashed value --->
<cfif isDefined("form.answer")>
	<cfif hash(form.answer) neq form.hashvalue>
Nope
	<cfelse>
Yay!
	</cfif>
</cfif>
```
Hopefully that might help someone!
