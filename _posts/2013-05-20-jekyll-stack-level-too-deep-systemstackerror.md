---
layout: post
status: publish
title: Jekyll - stack level too deep (SystemStackError)
date: '2013-05-20 14:57:46 +0100'
tags: ruby jekyll blog
---
The latest version of Jekyll has come out and I've been playing at converting my Wordpress blog again. Messing around with ruby versions and arguments (not being a rubyist!) keep getting an odd error.

Using 2.0.0-dev:

``` shell
$ ruby -r './wordpress.rb' \
  -e 'JekyllImport::WordPress.process({dbname:"db",user:"user"})'
/path/to/wordpress.rb:260: stack level too deep (SystemStackError)
```

Using 1.9.3-p327, the last character on line 260 had been causing pain:

``` shell
$ ruby -r './wordpress.rb' \
  -e 'JekyllImport::WordPress.process({dbname:"db",user:"user"})'
/path/to/.rbenv/versions/1.9.3-p327/lib/ruby/1.9.1/rubygems/custom_require.rb:36:in `require': /path/to/wordpress.rb:260: invalid multibyte char (US-ASCII) (SyntaxError)
/path/to/wordpress.rb:260: syntax error, unexpected $end
 }.delete_if { |k,v| v.nil? || v == '' }.to_yaml<strong>&not;</strong>
```

This was almost certainly introduced during copy and pasting between vim windows. C&amp;P death! Interesting fact those is ruby 2.0.0-dev hid that error, well, hid the detail of it anyway. I was originally [Paul Stamatiou's](http://paulstamatiou.com/how-to-wordpress-to-jekyll) version, but, am now trying the version from the beta gem.

Also note that I built the gem manually before I did all this:

``` shell
git clone git://github.com/jekyll/jekyll-import.git
gem build jekyll-import.gemspec
gem install ./jekyll-import-0.1.0.beta1.gem
```

I did have some other hacks from [icebreaker](https://gist.github.com/icebreaker/303570) and myself in the last version, so, I'll get those in and hopefully publish it.
