---
layout: post
title: "Build your own Yum repository"
author: iwinter
tags:
  - yum
  - packagemanagement
status: publish
type: post
published: true
original: https://dev.venntro.com/2015/06/build-your-own-yum-repo/
summary: >
  A summary of how to build and manage your own Yum repository to get
  around the challenges of packages being removed, or, to tie specific
  versions and easier upgrade paths.
---

One of the challenges we've had over the years is installing packages
on RHEL and CentOS that either go away or are removed from major
repos (EPEL/REMI/IUS), or, for things where we want to tie to a
specific version of a package and maintain easy upgrade ability.

There are many ways to skin this cat, but, this is the way we chose.
No cats were harmed in the making of this repository.

## Create

First off, you'll need to create some directories for your repo and
packages to live in. You'll need to do this for each version and
architecture you're going to support. In this case it's RHEL/CentOS
only and versions 5 and 6.

{% highlight bash %}
$ mkdir -p /path/to/yum/reponame/{5,6}/{i386,x86_64}
{% endhighlight %}

If you had multiple architectures & OS' you might have:

{% highlight bash %}
/path/to/yum/reponame/CentOS/{5,6}/{i386,x86_64}
/path/to/yum/reponame/Fedora/{SRPMS,i386,x86_64}
{% endhighlight %}

Directories are up to you really depending on what you want to store.

You'll also need the `createrepo` package, and `repoview` if you want
an HTML setup as well:

{% highlight bash %}
yum install createrepo repoview
{% endhighlight %}

We've got a script that's specific to our install and as I said assumes
only one architecture, but, you can tweak as you need. The advantage of
the script in this format is you can run it from wherever, say you have
a common shared bin directory for instance.

{% highlight bash %}
$ cat ./build
#!/bin/bash

if (( EUID != 0 )); then
	echo "You must be root to run this command"
	exit 1
fi

[[ -n "$1" ]] && YUMREPO="/path/to/yum/$1" || YUMREPO=$( find /path/to/yum -mindepth 1 -maxdepth 1 -type d )

for REPO in $YUMREPO; do
	REPONAME=$( basename $REPO )
	for RELEASEARCH in $( find $REPO/[0-9]* -mindepth 1 -maxdepth 1 -type d 2> /dev/null ); do
		RELEASEVER=$( basename `dirname $RELEASEARCH` )
		BASEARCH=$( basename $RELEASEARCH )
		echo "Building $REPONAME RHEL $RELEASEVER ($BASEARCH): $RELEASEARCH"
		cd $RELEASEARCH
		createrepo -d .
		repoview -f -t "$REPONAME yum repo: RHEL $RELEASEVER $BASEARCH" .
		echo
	done
done
{% endhighlight %}

Once you've run it, you'll have your repo.

## Add packages & update

Now it's built, you can add packages. Put simply, chuck an RPM in the
right folder and rebuild.

## Use it

Once you've create the repository it's easy to then go ahead an grab a
package from it.

On each server you want it to be available on, create a new repo file.
We use puppet to ensure this exists where it needs to. Note that this
example is only for RHEL/CentOS 6.

{% highlight bash %}
$ more /etc/yum.repos.d/reponame.repo
[reponame]
name=RepoName
gpgcheck=0
baseurl=http://yum.domain.com/6/$basearch/
enabled=1
{% endhighlight %}

Once you've added that, update Yum:

{% highlight bash %}
$ yum clean all
{% endhighlight %}

Then, go ahead and search for your package. We'll use ImageMagick as an
example:

{% highlight bash %}
$ yum --disablerepo=* --enablerepo=reponame search package
{% endhighlight %}

What we're doing in the above is ensuring only our repo responds. After
this point you can install or do whatever you need to.

<em>Orginally published at <a href="{{ page.original }}">dev.venntro.com</a></em>
