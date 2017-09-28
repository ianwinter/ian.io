---
layout: post
status: publish
title: Useful sed command


date: '2009-06-08 15:29:29 +0100'
date_gmt: '2009-06-08 14:29:29 +0100'
tags:
- sed
- unix
- commands
- shell script
---
Posting this more as a reminder to myself than anything else, but, if you need to replace all instances of the word old in a file with the word new this command is really handy!
```
sed -i 's/old/new/g' file.txt
```
