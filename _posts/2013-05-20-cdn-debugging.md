---
layout: post
status: publish
title: CDN Debugging
date: '2013-05-20 12:28:41 +0100'
tags: cdn akamai limelight debugging
---
Working with a <a href="http://en.wikipedia.org/wiki/Content_delivery_network">CDN</a> can be a whole world of pain sometimes, especially when you're trying to figure out what the heck is going on.
Firstly, CDN's quite often will have a pair of <a href="http://en.wikipedia.org/wiki/Points_of_presence">POP's</a>. Someone like LLNW will return you this:

``` shell
$ host media.limelight.com
media.limelight.com is an alias for llnw.vo.llnwd.net.
llnw.vo.llnwd.net has address 87.248.213.101
llnw.vo.llnwd.net has address 178.79.207.22
llnw.vo.llnwd.net has IPv6 address 2a02:3d0:600:15:230:48ff:feda:d0c0
llnw.vo.llnwd.net has IPv6 address 2a02:3d0:600:212:225:90ff:fe4a:ef16
```

Each POP could return you different content. Whilst not recommended you could target a POP directly like this:

``` shell
curl -H 'Host: media.limelight.com' -H 'X-LDebug:2' \
  -I http://87.248.213.101/designimages/limelight-content-delivery.jpg
```

LimeLight Networks offer some debugging via a header which is shown in the command above, "X-LDebug:2". If you make a call with it in you'll see some extra X-Cache information:

``` shell
$ curl -H 'Host: media.limelight.com' -H 'X-LDebug:2' \
  -I http://87.248.213.101/designimages/limelight-content-delivery.jpg
HTTP/1.1 200 OK
Server: Apache
Cache-Control: max-age=3600
X-Server-Name: la-c1-r3-u30-b1
Content-Type: image/jpeg
Via: 1.1 la-c1-r2-u20-b2:3128 (squid)
X-Cache: MISS from cds1020.iad.llnw.net
X-Cache: MISS from cds822.lga.llnw.net
Age: 308
Date: Mon, 20 May 2013 11:15:05 GMT
Last-Modified: Thu, 08 Mar 2012 22:18:12 GMT
Expires: Mon, 20 May 2013 12:09:57 GMT
X-Cache: HIT from cds163.lon.llnw.net
Content-Length: 5486
X-Cache: MISS from cds761.lon.llnw.net
Connection: keep-alive
```

Akamai are a much larger network and as such can be a bit more complex to find POPs, but, they also offer a lot more in terms of debugging. Most of this is lifted from [Mesmor](http://mesmor.com/2012/03/18/akamai-pragma-debug-headers/).
Akamai provide an extension for both Internet Explore and Firefox which can be found in the [control panel](https://control.akamai.com/portal/content/akaServiceSupport/edgesuite/edgesuite_booster.jsp. You can also quickly see the information from an Akamai asset using curl is:

``` shell
curl -I -H "Pragma: akamai-x-cache-on, akamai-x-cache-remote-on,
akamai-x-check-cacheable, akamai-x-get-cache-key, akamai-x-get-extracted-values,
akamai-x-get-nonces, akamai-x-get-ssl-client-session-id, akamai-x-get-true-cache-key,
akamai-x-serial-no" http://www.website.com/style.css
```

Some explanation of the results:

**X-Cache : Cache State**

``` shell
**X-Cache TCP_MEM_HIT from a118-214-191-15 (AkamaiGHost/6.7.0.2-9183648)**
a118-214-191-15      : Akamai IP Identifier (The Akamai
                       server that serves this request)
TCP_HIT              : The object was fresh in cache and
                       object from disk cache.
TCP_MISS             : The object was not in cache, server
                       fetched object from origin.
TCP_REFRESH_HIT      : The object was stale in cache and we
                       successfully refreshed with the origin
                       on an If-modified-Since request.
TCP_REFRESH_MISS     : Object was stale in cache and refresh
                       obtained a new object from origin
                       in response to our IF-Modified-Since request.
TCP_REFRESH_FAIL_HIT : Object was stale in cache and we
                       failed on refresh (couldn't reach origin)
                       so we served the stale object.
TCP_IMS_HIT          : IF-Modified-Since request from client
                       and object was fresh in cache and served.
TCP_NEGATIVE_HIT     : Object previously returned a "not found"
                       (or any other negatively cacheable response)
                       and that cached response was a hit for this
                       new request.
TCP_MEM_HIT          : Object was on disk and in the memory
                       cache. Server served it without hitting
                       the disk.
TCP_DENIED           : Denied access to the client for whatever reason.
TCP_COOKIE_DENY      : Denied access on cookie authentication
                       (if centralized or decentralized authorization
                       feature is being used in config).
```

**X-Cache-Key : Internal Cache Key**

``` shell
**X-Cache-Key /L/1745/15811/3h/www.website.com/common/css/style.css**
15811           : Akamai CP Code
3h              : Cache TTL
www.website.com : Origin Server Name
```

**X-Check-Cacheable : Cacheability**

This determines if the request object is cache-able as set in your Akamai configuration.
You can read more about&nbsp;<a href="http://www.cdnplanet.com/blog/how-test-your-cdn-before-go-live/">CDN testing pre-live</a> and also accessing some other <a href="http://www.cedexis.com/blog/fun-with-headers/">providers headers</a>.
