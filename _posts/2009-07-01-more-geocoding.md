---
layout: post
status: publish
title: More Geocoding


date: '2009-07-01 23:18:37 +0100'
date_gmt: '2009-07-01 22:18:37 +0100'
tags:
- geocoding
- perl
---
More geocoding fun today, this time done with perl. For what I want to do, convert OS Grid easting and northing into latitude and longitude using an API, like Yahoo! was going to take ages due to throttling. Turns out that in perl using the handy <a href="http://search.cpan.org/~pkent/Geography-NationalGrid-1.6/" target="_blank">Geography-NationalGrid-1.6</a> CPAN package it's a lot easier and gives for most cases a result very close together.
Here's a quick example:
```
#!/usr/bin/perl
use Geography::NationalGrid;
my $point1 = new Geography::NationalGrid( 'GB',
Easting => '385600',
Northing => '801900'
);
print "Latitude " . $point1->latitude . " Longitude " . $point1->longitude . "\n";
```
You can also give it a grid reference like TH 1234 1234 or a latitude and longitude.
