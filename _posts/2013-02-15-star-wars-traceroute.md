---
layout: post
status: publish
title: Star Wars traceroute
date: '2013-02-15 13:55:20 +0000'
tags: linux traceroute shell starwars
---
Kind of old news now, but, I'll post this anyway.

Take a read of [this post](http://www.theregister.co.uk/2013/02/15/star_wars_traceroute/). To find out what it's all about and how it was done. If you don't understand or can't be bothered, here's the output.

``` shell
$ traceroute 216.81.59.173
traceroute to 216.81.59.173 (216.81.59.173), 64 hops max, 52 byte packets
 1 192.168.50.1 (192.168.50.1) 0.572 ms 0.375 ms 0.364 ms
 2 84.19.53.18 (84.19.53.18) 0.575 ms 0.565 ms 0.527 ms
 3 84.19.53.2 (84.19.53.2) 2.520 ms 2.935 ms 2.440 ms
 4 88.211.81.53 (88.211.81.53) 2.662 ms 2.977 ms 2.623 ms
 5 v207.core1.lon1.he.net (216.66.80.177) 5.594 ms 10.048 ms 13.509 ms
 6 10gigabitethernet2-4.core1.par2.he.net (72.52.92.42) 9.420 ms 12.393 ms
 10gigabitethernet7-4.core1.nyc4.he.net (72.52.92.241) 71.061 ms
 7 10gigabitethernet2-3.core1.ash1.he.net (72.52.92.86) 81.497 ms 76.879 ms 84.098 ms
 8 10gigabitethernet1-2.core1.atl1.he.net (184.105.213.110) 96.704 ms 99.003 ms 99.772 ms
 9 216.66.0.26 (216.66.0.26) 95.045 ms 93.482 ms 103.896 ms
10 10.26.26.102 (10.26.26.102) 129.185 ms 127.454 ms 128.497 ms
11 episode.iv (206.214.251.1) 127.964 ms 130.673 ms 126.375 ms
12 a.new.hope (206.214.251.6) 132.574 ms 135.954 ms 132.619 ms
13 it.is.a.period.of.civil.war (206.214.251.9) 137.614 ms 134.545 ms 132.662 ms
14 rebel.spaceships (206.214.251.14) 136.608 ms 148.727 ms 136.672 ms
15 striking.from.a.hidden.base (206.214.251.17) 138.535 ms 130.344 ms 128.546 ms
16 have.won.their.first.victory (206.214.251.22) 131.485 ms 129.979 ms 128.568 ms
17 against.the.evil.galactic.empire (206.214.251.25) 129.952 ms 132.530 ms 134.179 ms
18 during.the.battle (206.214.251.30) 136.926 ms 137.240 ms 128.703 ms
19 rebel.spies.managed (206.214.251.33) 129.211 ms 129.687 ms 131.459 ms
20 to.steal.secret.plans (206.214.251.38) 137.336 ms 134.963 ms 137.010 ms
21 to.the.empires.ultimate.weapon (206.214.251.41) 137.164 ms 132.548 ms 136.727 ms
22 the.death.star (206.214.251.46) 138.797 ms 135.106 ms 131.307 ms
23 an.armored.space.station (206.214.251.49) 131.542 ms 127.945 ms 127.242 ms
24 with.enough.power.to (206.214.251.54) 132.804 ms 136.714 ms 136.465 ms
25 destroy.an.entire.planet (206.214.251.57) 137.520 ms 131.996 ms 132.086 ms
26 pursued.by.the.empires (206.214.251.62) 127.587 ms 128.695 ms 133.108 ms
27 sinister.agents (206.214.251.65) 132.133 ms 130.615 ms 131.129 ms
28 princess.leia.races.home (206.214.251.70) 136.134 ms 148.275 ms 147.835 ms
29 aboard.her.starship (206.214.251.73) 147.638 ms 141.807 ms 129.552 ms
30 custodian.of.the.stolen.plans (206.214.251.78) 140.750 ms 142.327 ms 150.433 ms
31 that.can.save.her (206.214.251.81) 150.344 ms 145.455 ms 139.564 ms
32 people.and.restore (206.214.251.86) 141.018 ms 133.512 ms 133.397 ms
33 freedom.to.the.galaxy (206.214.251.89) 135.804 ms 130.033 ms 131.854 ms
34 0-----i-------i-----0 (206.214.251.94) 131.347 ms 131.364 ms 136.892 ms
35 0------------------0 (206.214.251.97) 137.222 ms 138.468 ms 135.353 ms
36 0-----------------0 (206.214.251.102) 130.934 ms 131.678 ms 131.364 ms
37 0----------------0 (206.214.251.105) 131.165 ms 128.787 ms 132.123 ms
38 0---------------0 (206.214.251.110) 131.030 ms 130.483 ms 136.405 ms
39 0--------------0 (206.214.251.113) 136.736 ms 137.485 ms 139.646 ms
40 0-------------0 (206.214.251.118) 132.272 ms 129.410 ms 131.636 ms
41 0------------0 (206.214.251.121) 130.995 ms 137.218 ms 136.325 ms
42 0-----------0 (206.214.251.126) 138.280 ms 137.995 ms 138.286 ms
43 0----------0 (206.214.251.129) 136.456 ms 137.882 ms 137.453 ms
44 0---------0 (206.214.251.134) 132.797 ms 131.182 ms 132.678 ms
45 0--------0 (206.214.251.137) 131.026 ms 146.013 ms 134.406 ms
46 0-------0 (206.214.251.142) 136.977 ms 134.188 ms 131.634 ms
47 0------0 (206.214.251.145) 133.170 ms 132.137 ms 131.775 ms
48 0-----0 (206.214.251.150) 131.519 ms 130.287 ms 132.893 ms
49 0----0 (206.214.251.153) 133.129 ms 138.519 ms 136.406 ms
50 0---0 (206.214.251.158) 138.966 ms 146.695 ms 142.437 ms
51 0--0 (206.214.251.161) 143.186 ms 132.973 ms 133.305 ms
52 0-0 (206.214.251.166) 137.930 ms 135.482 ms 136.323 ms
53 00 (206.214.251.169) 137.449 ms 135.504 ms 136.256 ms
54 i (206.214.251.174) 136.883 ms 135.939 ms 131.455 ms
55 by.ryan.werber (206.214.251.177) 131.137 ms 131.221 ms 131.316 ms
56 when.ccies.get.bored (206.214.251.182) 131.330 ms 131.044 ms 132.570 ms
57 read.more.at.beaglenetworks.net (206.214.251.185) 131.069 ms 135.185 ms 136.913 ms
58 read.more.at.beaglenetworks.net (206.214.251.190) 138.096 ms * 138.574 ms
```
