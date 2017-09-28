---
layout: post
status: publish
title: Move objects Rackspace Cloud Files
date: '2013-08-20 10:51:54 +0100'
tags: rackspace cloud
---
I had a need to move a bunch of files to a new container in [Rackspace's Cloud Files](http://www.rackspace.co.uk/cloud/files). Using the handy API Python Bindings ([pyrax](https://github.com/rackspace/pyrax)) this is pretty easy.

``` python
#!/usr/bin/python2.7
import os
import sys
import pyrax
import pyrax.exceptions as exc
import pyrax.utils as utils
creds_file = os.path.expanduser(".rackspace_cloud_credentials")
pyrax.set_setting("identity_type", "rackspace")
pyrax.set_setting("region", "LON")
pyrax.set_credential_file(creds_file)
cf = pyrax.cloudfiles
oldcont = cf.get_container("old_container")
newcont = cf.get_container("new_container")
objects = oldcont.get_objects()
counter = 0
for obj in objects:
 cf.move_object(oldcont, obj.name, newcont)
 counter += 1
 print "[%d] %s moved from %s to %s" % (counter, obj.name, oldcont.name, newcont.name)
```

The credentials file, if you're wondering, looks like this:

```
[rackspace_cloud]
username = USERNAME
api_key = KEY
```

The follow up is to port this to ruby/[fog](https://github.com/fog/fog)/[rumm](https://github.com/rackerlabs/rumm) once I figure out ruby and get time :)
