---
layout: post
status: publish
title: Best way to search a MySQL database?


date: '2009-05-22 12:26:28 +0100'
date_gmt: '2009-05-22 11:26:28 +0100'
tags:
- mysql
- performance
- Work
- searching
---
Currently looking into a whole load of performance related issues with an application at work. Basically, how to make it go faster and support more users online at any given time.
Currently our setup has a MySQL 5.0.x master running with quite a few slaves. The setup is OK but isn't as scalable as we'd like. The master isn't suffering load wise so we don't need to look at distrubtion masters or get into pyramid replication which is just a bit too complex. The main hang up is searching. We have members, each member has loads of related details, photos, payments, messages, profile information etc and currently it's spread out over a number of tables. We've looked at sphinx, hadoop, solr / lucence but aren't sure if those kinds of things can be used for multi faceted objects. We don't want a plain text search because the data is mostly lots of numbers like height, age, hair colour all picked from lookup tables but maybe we can combine our data into some form of index that is better for searching? Currently we flatten the most searched data out into single tables (split on certain categorizations) and search those the details in them are updated regularly by triggered actions in the application. Memcache is an option as we use it to store data already.
MySQL 5.4 looks great in the performance enhancements it could bring but that's now not looking like it's going to be coming until around December. I hope Sun release it sooner however as the inital peek at it we've had seem to back up Sun's claims.
Any ideas / suggestions / experience would be appreciated. Comment or email me.
