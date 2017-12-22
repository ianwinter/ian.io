---
layout: post
title: Store SequelPro config in Dropbox
canonical_url: http://www.gigoblog.com/2014/05/19/store-sequel-pro-favorites-and-preferences-in-dropbox/
---

I use SequelPro as my MySQL GUI of choice. It has its quirks, but, it does the job well.

One of it's shortcomings however is the inability to share/sync/backup it's connections and settings. Even with the method I'm going to describe you'll still have to re-enter passwords to the keychain.

I tried creating a specific keychain for this so it could be moved around, and, whilst it works I don't know enough about the MacOS keychain to stop it prompting for my password every time I load it up - I suspect it's a permission/approval issue somewhere.

Create a folder on Dropbox to hold the files:

``` bash
mkdir -p ~/Dropbox/Apps/SequelPro
```

Copy over the plist files that you need:

``` bash
cp ~/Library/Application\ Support/Sequel\ Pro/Data/Favorites.plist ~/Dropbox/Apps/SequelPro/Data/Favorites.plist
cp ~/Library/Preferences/com.sequelpro.SequelPro.plist ~/Dropbox/Apps/SequelPro/Preferences/com.sequelpro.SequelPro.plist
```

Remove the plist files from the local locations:

``` bash
rm ~/Library/Application\ Support/Sequel\ Pro/Data/Favorites.plist
rm ~/Library/Preferences/com.sequelpro.SequelPro.plist
```

Now create symbolic links (symlinks) to the files in Dropbox:

``` bash
ln -s ~/Dropbox/Apps/SequelPro/Favorites.plist ~/Library/Application\ Support/Sequel\ Pro/Data/Favorites.plist
ln -s ~/Dropbox/Apps/SequelPro/com.sequelpro.SequelPro.plist ~/Library/Preferences/com.sequelpro.SequelPro.plist
```

**VERY IMPORTANT.** If you launch Sequel Pro without first running this command, it will overwrite all your copied plist files, reverting them back to the previous state. You must first delete the cached preference files for Sequel Pro:

``` bash
rm -Rf ~/Library/Caches/com.sequelpro.SequelPro
```

where `<username>` is the username reported by the `whoami` command in terminal.

_Originally posted by Chris Brewer at [Gigoblog](http://www.gigoblog.com/2014/05/19/store-sequel-pro-favorites-and-preferences-in-dropbox/)._

