---
layout: post
status: publish
title: eximstats failed

date: '2006-01-13 22:11:00 +0000'
date_gmt: '2006-01-13 22:11:00 +0000'
tags:
- linux
---
If you've got a Linux box running Cpanel/WHM and you've updated recently you may now have eximstats failed and also noticed some DBD::MySQL errors in the process. Quick fix which worked for me is:
<div class="code">/scripts/realperlinstaller --force DBD::mysql
/scripts/restartsrv_eximstats
/etc/init.d/chkservd restart</div>
