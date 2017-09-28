---
layout: post
status: publish
title: Rackspace Cloud (Mosso) DNS


date: '2009-06-12 22:59:57 +0100'
date_gmt: '2009-06-12 21:59:57 +0100'
tags:
- mosso
- rackspace
- cloud
- dns
---
Some people aren't aware that for <a href="http://www.rackspace.co.uk/cloud/servers " target="_blank">Rackspace Cloud Servers</a> they also offer DNS which is pretty handy. The setup however is a little odd and the interface different to any other DNS/zone editor I've used. There's a number of KB entries under The Rackspace Cloud control panel as well.
A quick guide on how to enter the details to get it to work. If you login, access your server and click DNS you'll first need to add the domain name, in these examples domain.com. Once done click on the domain and then you can add away. You can't edit, well if you call delete and add editing then yes you can.
So, for an A record:
```
Name: domain.com
Content: 1.2.3.4
Priority: 10
TTL: 300
Type: A
```
And a CNAME record:
```
Name: cname.domain.com
Content: another.domain.co.uk
Priority: 10
TTL: 300
Type: CNAME
```
For sub-domains you have to put the full domain in rather than just the sub-domain so:
```
Name: sub.domain.com
Content: 1.2.3.4 (or a CNAME)
Priority: 10
TTL: 300
Type: A
```
For MX records they say just use your IP as the content:
```
Name: domain.com
Content: 1.2.3.4
Priority: 10 (increment for additional MX entries)
TTL: 300
Type: MX
```
I think however you can also do this:
```
Name: domain.com
Content: 10 mail.domain.com (assuming you have a standard A record setup with IP called mail)
Priority: 10
TTL: 300
Type: MX
```
That's it.
