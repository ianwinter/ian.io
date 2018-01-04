---
layout: post
title: Upgrade kernel to mainline
tags: kernel security linux
---

Why should you upgrade your kernel? There's a bunch of reasons really. Bug fixes get included, they can be faster and add in new features.

There is however the huge caveat that not everyone can. Some things depend on specific kernels to work correctly.

Earlier this week a "bug" became public that has attracted a lot of attention and rightly so as it really is quite bad.

* [Meltdown, Spectre: The password theft bugs at the heart of Intel CPUs](https://www.theregister.co.uk/2018/01/04/intel_amd_arm_cpu_vulnerability/)
* [Rush to fix 'serious' computer chip flaws](http://www.bbc.co.uk/news/technology-42561169)
* [Meltdown and Spectre: How chip hacks work](http://www.bbc.co.uk/news/technology-42564461)

It's been covered by all the major companies as it affects anything with an Intel processor, and, actually has done for years. Here's some (not all):

* [AWS](https://aws.amazon.com/security/security-bulletins/AWS-2018-013/)
* [RHEL](https://access.redhat.com/security/vulnerabilities/speculativeexecution?sc_cid=701f2000000tsLNAAY&)
* [VMWare - ESX](https://www.vmware.com/us/security/advisories/VMSA-2018-0002.html)

http://elrepo.org/tiki/tiki-index.php

# Install elrepo
7: rpm -Uvh http://www.elrepo.org/elrepo-release-7.0-2.el7.elrepo.noarch.rpm
6: rpm -Uvh http://www.elrepo.org/elrepo-release-6-6.el6.elrepo.noarch.rpm

# Import key
rpm --import https://www.elrepo.org/RPM-GPG-KEY-elrepo.org

# Install mainline kernel
yum --disableexclude=kernel* --enablerepo=elrepo* install kernel-ml

# Change default kernel to newest
# https://www.thegeekdiary.com/centos-rhel-56-change-default-kernel-boot-with-old-kernel/
# [ WARN ] This will set option 0 regardless of what's there now
sed -i 's/default=[0-9]\{1\}/default=0/' /boot/grub/grub.conf

# Reboot
shutdown -r now

# Check
uname -a

# Keep 1 old kernel
# https://www.if-not-true-then-false.com/2012/delete-remove-old-kernels-on-fedora-centos-red-hat-rhel/
package-cleanup --oldkernels --count=1

# Remove devel and header packages
# https://ma.ttias.be/removing-a-package-without-its-dependencies-in-centos-or-rhel/
rpm -e --nodeps kernel-headers-{VERSION}

# Install new headers
yum --disableexclude=kernel* --enablerepo=elrepo* install kernel-ml-headers kernel-ml-devel


6:
lsit kernels
 $ sudo grep '^[[:space:]]*kernel' /boot/grub/grub.conf

7:

edit /boot/grub2/grub.cfg
grub2-mkconfig -o /boot/grub2/grub.cfg
