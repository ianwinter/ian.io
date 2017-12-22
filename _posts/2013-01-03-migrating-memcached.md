---
layout: post
title: "Migrating memcached"
tags:
- memcached
- migration
- tcpcopy
status: publish
type: post
published: true
canonical_url: https://dev.venntro.com/2013/01/migrating-memcached/
---

As part of the pre-Christmas push we rolled out a series of
infrastructure improvements including both new additional servers and
new servers to replace old ones. One set of servers to be upgraded were
our cache servers. These have two roles day-to-day, firstly they're our
[Akamai][akamai] (CDN) origin servers and secondly they store our
primary memcached instances.

Each server has multiple instances of memcached running on different
ports. There are multiple sets for both the "data" and "view" segments.
Each of the new servers has 64GB RAM.

## Migration Challenges

The main challenges when planning to migrate the data is mitigating the
impact on the platform. memcached is a core element. It's used to store
HTML fragment caches and various parts of both site and member
data.

There is a tight line between getting the migration completed versus
keeping the application as quick as possible during the whole procedure.

## Approaches

There are a few approaches which are briefly outlined below:

* **Big Bang**. Update all the configuration files and simply push that
  code live. This has a huge hit on performance as all the new instances
  are cold, it is however the quickest option.
* **Migration instance by instance.** Each instance would be moved and
  relevant configuration files pushed live. This way only 1 or X
  instances is "cold". This limits impact, but, also takes time as only
  1 instance can be done at any one time.
* **Write to both.** The application code could be altered to make all
  writes to both the existing set of instances and the new. This isn't
  great as we'd need a full QA cycle to validate the code works. It's
  also something we'd have to implement then pull back out down the
  line.
* **Mirror traffic.** Similar to the above, but, this time lower down
  the stack. Essentially duplicate all the TCP level traffic so it warms
  in parallel and keeps both instances in sync meaning new writes,
  deletes occur on both existing and new sets.

## Solution

In the end the best solution which minimised the impact to the platform
was to mirror all the traffic to the existing instances across to the
new instances. `tcpcopy` helped us to get this done. As well as migrating
data, `tcpcopy` could be used to load test, stress test and benchmark.

The plan involved running the `tcpcopy` for 24 hours, see how full the new
instances reached then decided if another 24 hours should elapse before
the code changes to point at new instances are deployed.

There would be some data "missing" however all freqently used data keys,
core and shared information will all exist and as such mitigate the
performance concerns.

### `tcpcopy`

Installing [`tcpcopy`][tcpcopy] is pretty straight forward. Assuming all
requirements are in place:

{% highlight bash %}
wget --no-check-certificate \
  https://github.com/downloads/wangbin579/tcpcopy/tcpcopy-0.6.5.tar.gz && \
  tar xvfz tcpcopy-0.6.5.tar.gz && cd tcpcopy-0.6.5 && \
  ./configure && make && make install
{% endhighlight %}

Once that's installed on your target server (new cache servers) you'll
need to setup the iptables rules. Note there are a couple of extra's
here to ensure that connections from the localhost and from the office
IP were not queued to allowing monitoring.

{% highlight bash %}
modprobe ip_queue;
iptables -A OUTPUT -o lo -j ACCEPT;
iptables -A OUTPUT -d 1.2.3.4/32 -j ACCEPT;
iptables -A OUTPUT -p tcp --match multiport --sports 11211:11212 -j QUEUE;
{% endhighlight %}

You can then fire up the intercepter, the `-d` option backgrounds the
process as a daemon.

{% highlight bash %}
intercept -d
{% endhighlight %}

Next you'll need to head over to the source servers. In our case this is
the existing cache servers.

{% highlight bash %}
tcpcopy -d -x 11211-192.168.2.184:11211
tcpcopy -d -x 11212-192.168.2.184:11212
{% endhighlight %}

The format of the copy is `localport-targetip:targetport`.  You can
tweak with more options as well, see the [manual][tcpcopy-manual] for
full details.  You can also include logging if needed by using `-l
/path/to/log`.

### Monitoring

In order to see how the copies were going 3 monitoring methods were used.

#### Direct monitoring via `telnet`

{% highlight text %}
[root@server ~] telnet 0 11211
Trying 0.0.0.0...
Connected to 0.
Escape character is '^]'.
stats
STAT pid 1234
STAT uptime 1234
STAT time 111111111
...
{% endhighlight %}

#### Ruby key dumper

This small [Ruby key dumper][key-dumper] allows you to dump out a
selection of the keys. It's not great and memcached doesn't allow a full
dump of keys in anyway. It allows you to see what's there and validate
it's content though.

#### Overall instance stats from `memcache-top`

This is a perl program available on [google code][mc-top] which allows
you monitor a given set of instances, much like regular top.

<em>Orginally published at <a href="{{ page.canonical_url }}">dev.venntro.com</a></em>

[akamai]: http://www.akamai.com/
[tcpcopy]: https://github.com/wangbin579/tcpcopy
[tcpcopy-manual]: https://github.com/downloads/wangbin579/tcpcopy/TCPCopy_0.6.5_Manual.pdf
[key-dumper]: https://gist.github.com/1365005
[mc-top]: http://code.google.com/p/memcache-top/
