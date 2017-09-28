---
layout: post
status: publish
title: Got bad file info - path disallowed for security reasons

date: '2005-10-06 13:13:00 +0100'
date_gmt: '2005-10-06 13:13:00 +0100'
---
I use <a href="http://www.torrentflux.com">TorrentFlux</a> on one of my linux servers to consume torrents, recently after upgrading to MySQL noticed a bug, turns out it was unrelated to the the upgrade but in fact a bug in the .torrent file in question.
If you get:
"Got bad file info - path disallowed for security reasons"
there is a problem inside the torrent file, probably files starting with a . , fine on linux but not on windoze.
The solution is to go and edit on of the files. Go into your BitTornado directory, usually: /usr/local/TF_BitTornado/BitTornado/BT1/btformats.py 
Change line 7 from:<br>
reg = compile(r'^[^/\.~][^/\]*$')
To:<br>
reg = compile(r'^[^/\.~][^/\]*$|^$')
