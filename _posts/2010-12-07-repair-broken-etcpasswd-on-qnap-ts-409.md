---
layout: post
status: publish
title: Repair broken /etc/passwd on QNAP TS-409


date: '2010-12-07 23:10:01 +0000'
date_gmt: '2010-12-07 23:10:01 +0000'
tags:
- security
- qnap
- ts-409 pro
- howto
---
So, I was messing around with PS1 prompts on my QNAP TS-409 Pro and decided to add it into the .bash_profile. The shell wasn't BASH so I edited the /etc/passwd to change it, however in my haste I hit save and quit with the shell being /etc/bas - missing an h.
The result is you cannot login via SSH. As only the admin user can login remotely this is a bit of an issue.
A lot of frantic Googling led me to a post on the <a href="http://forum.qnap.com/viewtopic.php?f=35&amp;t=5838">QNAP forum</a> with a guide on how to fix it. I'm going to repeat these steps here as well with my modifications. I have a 4 disk RAID5 and what I noticed was that when I hot plugged all 4 drives back in the array rebuilt itself and basically kicked me off again.
&mdash;&nbsp;Shutdown the system and remove all drives
&mdash;&nbsp;Boot, login using telnet to port 13131 (ensure you enable this via the web ui before shutdown)
&mdash;&nbsp;Hot plug 3 of the 4 drives (assuming RAID5)
&mdash;&nbsp;Assemble the system RAID partition:
```
# mdadm -A /dev/md9 /dev/sda1 /dev/sdb1 /dev/sdc1 --run
# mkdir /mnt/HDA_ROOT
# mount /dev/md9 /mnt/HDA_ROOT
# cd /mnt/HDA_ROOT/.config
```
&mdash;&nbsp;edit the passwd file and correct any mistakes
```
# halt (shuts the unit down)
```
&mdash;&nbsp;plug in 4th drive
&mdash;&nbsp;power up
Turns out I hadn't noticed the .profile file which I could have changed and saved a world of pain!
