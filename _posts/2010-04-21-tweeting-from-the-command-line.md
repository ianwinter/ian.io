---
layout: post
status: publish
title: Tweeting from the command line


date: '2010-04-21 17:22:27 +0100'
date_gmt: '2010-04-21 16:22:27 +0100'
tags:
- twitter
- scripts
- bash
---
I read an article over at <a href="http://osxdaily.com/2010/04/20/post-a-twitter-update-via-the-command-line/" target="_blank">OS X Daily</a> about posting a tweet from the command line using the twitter API. Taking this one step further here's a handy little script that'll even stop you posting nothing and stuff that's too long.
``` bash

#!/bin/bash
TWEET=$1
TWEETLEN=${#TWEET}
if [ $TWEETLEN -eq 0 ] || [ $TWEETLEN -gt 140 ]; then
	if [ $TWEETLEN -gt 140 ]; then
		let EXTRA=$TWEETLEN-140
		echo "Usage: tweet \"message\" (140 chars or less, you're $EXTRA over)"
	else
		echo "Usage: tweet \"message\" (140 chars or less)"
	fi
	exit 1
else
	curl -u username:password -d status="$1" http://twitter.com/statuses/update.xml
fi
exit 0

```
Save the file as "tweet" and make sure it's in the path with executable permissions of 755.
``` shell
chmod 755 tweet
```
