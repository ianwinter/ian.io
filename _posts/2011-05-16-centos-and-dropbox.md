---
layout: post
status: publish
title: CentOS and Dropbox


date: '2011-05-16 22:21:17 +0100'
date_gmt: '2011-05-16 21:21:17 +0100'
tags:
- centos
- dropbox
- sync
---
Just because I can really I setup Dropbox syncing on one of my CentOS 5 servers. It's a little bit of a faff, but, not that hard. The <a href="http://wiki.dropbox.com/TipsAndTricks/TextBasedLinuxInstall">official guide</a> is online, but, this is a shorten version and might not cover everything but it worked on my headless VM just fine.
First up su to whichever user you want to install Dropbox as. You'll need two terminal's open so fire them both up now. You'll also need lynx, gcc and python installed so if you don't have, sort that out with yum.
In your home directory grab the correct binary for you (x86 or x86_64).
```

wget -O dropbox.tar.gz http://www.dropbox.com/download/?plat=lnx.x86
wget -O dropbox.tar.gz http://www.dropbox.com/download/?plat=lnx.x86_64

```
Now unpack it and start it up in your first terminal window.
```

tar xvfz dropbox.tar.gz
~/.dropbox-dist/dropboxd

```
Now in your second terminal go to http://dropbox.com using lynx (lynx http://dropbox.com) and login, then using the key shortcut "g" paste in the URL that your first window should now be prompted you to go to. This is to register the machine with your Dropbox account. You'll need to scroll down the page to re-enter your password. Once done the first window should say hello to you and it'll quit.
Next up sort out the init.d script. Put the following in /etc/init.d/dropbox and chown 755 it when you're done.
```

# chkconfig: 345 85 15
# description: Startup script for dropbox daemon
#
# processname: dropboxd
# pidfile: /var/run/dropbox.pid
# config: /etc/sysconfig/dropbox
#
### BEGIN INIT INFO
# Provides: dropboxd
# Required-Start: $local_fs $network $syslog
# Required-Stop: $local_fs $syslog
# Should-Start: $syslog
# Should-Stop: $network $syslog
# Default-Start: 2 3 4 5
# Default-Stop: 0 1 6
# Short-Description: Start up the Dropbox file syncing daemon
# Description:       Dropbox is a filesyncing sevice provided by dropbox.com
#                    This service starts up the dropbox daemon.
### END INIT INFO
# Source function library.
. /etc/rc.d/init.d/functions
# To configure, add line with DROPBOX_USERS="user1 user2" to /etc/sysconfig/dropbox
# Probably should use a dropbox group in /etc/groups instead.
[ -f /etc/sysconfig/dropbox ] &amp;&amp; . /etc/sysconfig/dropbox
prog=dropboxd
lockfile=${LOCKFILE-/var/lock/subsys/$prog}
config=${CONFIG-/etc/sysconfig/dropbox}
RETVAL=0
start() {
    echo -n $"Starting $prog"
    if [ -z $DROPBOX_USERS ] ; then
        echo -n ": unconfigured: $config"
        echo_failure
        echo
        rm -f ${lockfile} ${pidfile}
        RETURN=6
        return $RETVAL
    fi
    for dbuser in $DROPBOX_USERS; do
        daemon --user $dbuser /bin/sh -c "/home/$dbuser/.dropbox-dist/dropboxd&amp;"
    done
    RETVAL=$?
    echo
    [ $RETVAL = 0 ] &amp;&amp; touch ${lockfile}
    return $RETVAL
}
status() {
    for dbuser in $DROPBOX_USERS; do
        dbpid=`pgrep -u $dbuser dropbox | grep -v grep`
        if [ -z $dbpid ] ; then
            echo "dropboxd for USER $dbuser: not running."
        else
            echo "dropboxd for USER $dbuser: running (pid $dbpid)"
        fi
    done
}
stop() {
    echo -n $"Stopping $prog"
    for dbuser in $DROPBOX_USERS; do
        killproc /home/$dbuser/.dropbox-dist/dropbox
    done
    RETVAL=$?
    echo
    [ $RETVAL = 0 ] &amp;&amp; rm -f ${lockfile} ${pidfile}
}
# See how we were called.
case "$1" in
    start)
        start
        ;;
    status)
        status
        ;;
    stop)
        stop
        ;;
    restart)
        stop
        start
        ;;
    *)
        echo $"Usage: $prog {start|status|stop|restart}"
        RETVAL=3
esac
exit $RETVAL

```
Edit /etc/sysconfig/dropbox and add the following line to match your linux account where you've installed Dropbox.
```

DROPBOX_USERS="linux_user_name"

```
Make sure it starts on boot.
```

chkconfig dropbox on.

```
That's it. Enjoy.
