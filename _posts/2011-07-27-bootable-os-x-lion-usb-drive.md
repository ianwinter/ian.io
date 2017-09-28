---
layout: post
status: publish
title: Bootable OS X Lion USB Drive


date: '2011-07-27 14:16:06 +0100'
date_gmt: '2011-07-27 13:16:06 +0100'
tags:
- mac
- osx
- lion
- install
---
This has been done to death on the vast expanse of the interweb, but, I'm blogging it anyway more for my own reference than anything else.
OS X Lion is distributed via the App Store however it's really easy to make a USB drive that's bootable which you can use to install Lion from.
<ol>
<li>Load up Mac App Store, login, purchase and download OS X Lion.</li>
<li>Once downloaded, open Finder and go to the Applications and locate the "Install Max OS X Lion" file</li>
<li>Right click on the downloaded file and select "Show Package Contents".</li>
<li>Go to "Contents", then inside the "SharedSupport" folder and you&rsquo;ll find a file titled "InstallESD.dmg". Copy this over to the desktop. Make sure you copy and paste rather than drag &amp; drop this file.</li>
<li>Plugin the USB drive you want to use. 4GB is enough, but, I'd do it on an 8GB one. I used <a href="http://www.amazon.co.uk/gp/product/B0047T6XPQ/ref=noref?ie=UTF8&amp;s=computers&amp;psc=1">one of these</a>&nbsp;with Amazon's frustration free packaging, which rocks!.</li>
<li>Open up "Disk Utility" and drag InstallESD.dmg from wherever you copied it to in to the left-hand sidebar. Select the attached USB from left side and click on "Partition" tab.</li>
<li>Select "1 Partition" from the Volume Scheme dropdown menu. Choose "Mac OS Extended (Journaled)" from the left.</li>
<li>Now click on "Option" at the bottom. Select "GUID Partition Table" and press OK. Now click on Apply at the bottom right. Be warned this is will erase all data on your USB flash drive.</li>
<li>Once it's done formatting click on&nbsp;"Restore" (it&rsquo;s right next to where you clicked on "Partition" in Step 6).</li>
<li>Choose the USB drive you plugged-in in Step 5 as "Destination" with the InstallESD.dmg file as "Source".</li>
<li>Click Restore and type in your password. This will create the Lion bootable USB flash drive.</li>
</ol>
<div>After it's done, you're pretty much set. You can either carry on and install Lion or you can reboot with your USB drive plugged in and whilst holding down the "Option" key select it and boot up to the installer.</div>
Also, if you don't back up your machine and loose everything, don't blame me. Good article on backing up and more specifically cloning can be found over on <a href="http://reverttosaved.com/2011/07/19/mac-os-x-users-clone-your-macs-before-installing-lion/">Craig Grannell's</a> site.
Thanks to articles on <a href="http://mashable.com/2011/07/20/lion-clean-install-guide/">Mashable</a>, <a href="http://www.informationweek.com/byte/howto/personal-tech/desktop-os/231002156">Information Week</a> and <a href="http://www.redmondpie.com/how-to-make-your-own-os-x-lion-bootable-usb-flash-drive-tutorial/">Redmond Pie</a>.
