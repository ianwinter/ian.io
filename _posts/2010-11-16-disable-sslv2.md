---
layout: post
status: publish
title: Disable SSLv2


date: '2010-11-16 13:45:10 +0000'
date_gmt: '2010-11-16 13:45:10 +0000'
tags:
- apache
- apache2
- ssl
---
If you're running a site with SSL you really need to turn SSLv2 off. The file you'll want to edit is /etc/httpd/conf.d/ssl.conf - it might be in a different location, but, shouldn't be hard to find. The two lines you want to make sure you have are:
```

SSLProtocol -ALL +SSLv3 +TLSv1
SSLCipherSuite ALL:!aNULL:!ADH:!eNULL:!LOW:!EXP:RC4+RSA:+HIGH:+MEDIUM

```
You can also test this once you've made the changes:
```

openssl s_client &ndash;ssl2 &ndash;connect virtualhost:443
openssl s_client &ndash;ssl3 &ndash;connect virtualhost:443

```
