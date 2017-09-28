---
layout: post
status: publish
title: Sphinx Multi-Value Attributes


date: '2009-07-06 11:03:18 +0100'
date_gmt: '2009-07-06 10:03:18 +0100'
tags:
- coldfusion
- java
- api
- sphinx
- mva
---
Sphinx allows you to create a multi-value attribute index which is great for document tags or categories. By default if you search across these the search is an OR match. If you want to run an AND match you must specify multiple filters rather than passing an array of values.
In ColdFusion using the Java API requires a little tweak as you can't pass a normal ColdFusion array to sphinx, you have to use a basic java type array. Tim Blair has a <a href="http://tim.bla.ir/tech/articles/cf-and-arrays-of-basic-java-types" target="_blank">good article</a> on how to create them but here's an example of calling the API.
This example will search for document tagged with either 100 or 200. Note the array creation method.
```
<cfset variables.sphinx = createobject("java", "org.sphx.api.SphinxClient").init()>
<cfset variables.sphinx.SetLimits(0, 10)>
<cfset variables.arrObj = createobject("java", "java.lang.reflect.Array")>
<cfset variables.jClass = createobject("java", "java.lang.Integer").TYPE>
<cfset variables.jArr = variables.arrObj.newInstance(variables.jClass, 2)>
<cfset variables.arrObj.setInt(variables.jArr, 0, 100)>
<cfset variables.arrObj.setInt(variables.jArr, 1, 200)>
<cfset variables.sphinx.SetFilter("tag", variables.jArr, FALSE)>
```
If you want to search for 100 AND 200 you'd do it like this:
```
<cfset variables.sphinx = createobject("java", "org.sphx.api.SphinxClient").init()>
<cfset variables.sphinx.SetLimits(0, 10)>
<cfset variables.sphinx.SetFilter("tag", 100, FALSE)>
<cfset variables.sphinx.SetFilter("tag", 200, FALSE)>
```
