---
layout: post
title: Running Transmission on a QNAP TS-409
---

I've got, and have had for many years, a QNAP TS-409. It's not great, efficient or pretty but it does the job. If you find yourself needing to download a legal torrent, say CentOS, but don't want to watch it you can run [Transmission](https://transmissionbt.com/) on the QNAP.

First up, you'll need to get `ipkg` installed. To that login to your admin interface and install the Optware QPKG plugin. It should be one of the default ones available.

You'll then need to be able to SSH on to the QNAP server itself (I'm not going to cover setting that up, but, look in the admin - should be pretty simple).

# Installation

``` shell
$ ipkg update
$ ipkg install transmission
```

Once you've got transmission installed, fire it up to create a config file then kill it (for now).

``` shell
$ transmission-daemon
$ killall transmission-daemon
$ mv /root/.config/transmission-daemon /opt/etc/transmission
```

You then need to change a setting, I found this via Google at the time and honestly don't remember why I had to set it.

``` shell
$ export EVENT_NOEPOLL=0
```

You're now at the point to fire it up.

``` shell
$ transmission-daemon -g /opt/etc/transmission
```

If all worked as it should, transmission should now be running on `http://0.0.0.0:9091/transmission/web/` - replace `0.0.0.0` with whatever your NAS IP actually is.

# Upgrade

Upgrading should be pretty simple, though, I'm not sure if the package will get upgraded as the 409 is _really_ old now.

``` shell
$ killall transmission-daemon
$ ipkg update
$ ipkg upgrade
$ transmission-daemon -g /opt/etc/transmission
$ transmission-daemon -V
```

Enjoy.

