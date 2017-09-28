---
layout: post
status: publish
title: Migrate from RPM nginx to source compiled nginx
date: '2013-08-22 13:10:05 +0100'
tags: nginx linux
---
If you've installed nginx via rpm/yum you'll be a bit stuck if you then decide you want to add some modules. In order to add modules, or, use something else you need to compile from source.

Below are the steps I took to get this going. This is on CentOS 6.4.

You'll need to map things to where they were if you want to keep the exact same locations.

``` shell
for i in nginx.conf nginx.pid client_body_temp proxy_temp fastcgi_temp uwsgi_temp scgi_temp; do echo "$i:"; locate -b $i; done
```

You'll need some pre-requisites.

``` shell
sudo yum -y install pcre pcre-devel libxml2 libxml2-devel curl curl-devel httpd-devel unzip openssl openssl-devel;
wget http://nginx.org/download/nginx-1.4.2.tar.gz;
wget https://github.com/pagespeed/ngx_pagespeed/archive/release-1.6.29.5-beta.zip;
wget https://dl.google.com/dl/page-speed/psol/1.6.29.5.tar.gz;
tar xfz nginx-1.4.2.tar.gz;
tar xfz 1.6.29.5.tar.gz;
unzip release-1.6.29.5-beta;
mv psol ngx_pagespeed-release-1.6.29.5-beta;
```

Once you're all unpacked you can get building.

``` shell
cd nginx-1.4.2;
./configure \
--prefix=/usr \
--conf-path=/etc/nginx/nginx.conf \
--pid-path=/var/run/nginx.pid \
--error-log-path=/var/log/nginx/error.log \
--http-log-path=/var/log/nginx/acces.log \
--http-client-body-temp-path=/var/cache/nginx/client_body_temp \
--http-proxy-temp-path=/var/cache/nginx/proxy_temp \
--http-fastcgi-temp-path=/var/cache/nginx/fastcgi_temp \
--http-uwsgi-temp-path=/var/cache/nginx/uwsgi_temp \
--http-scgi-temp-path=/var/cache/nginx/scgi_temp \
--with-http_gzip_static_module \
--with-http_addition_module \
--with-http_stub_status_module \
--add-module=../ngx_pagespeed-release-1.6.29.5-beta;
make;
sudo make install;
```

Now you've installed it, replacing the binary in place without dropping traffic.

``` shell
sudo kill -s USR2 `cat /var/run/nginx.pid`;
```

Now, I did try compiling this all with mod security 2.7.5 but get segfaults all over. Needs more investigation to figure out why!
