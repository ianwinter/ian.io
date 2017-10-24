---
layout: post
title: "Automating our site build using Jekyll, Travis CI, S3 & CloudFront"
author: iwinter
tags:
  - jekyll
  - s3
  - cloudfront
  - travis-ci
status: publish
type: post
published: true
summary: >
  An overview of how and why we choose to automate the build of the dev.venntro.com site
  and move it to a different hosting solution.
---
## Where the site was

The site was hosted on [GitHub Pages][gh-pages], which is based on Jekyll and used a
[custom domain](https://help.github.com/articles/using-a-custom-domain-with-github-pages/). Originally
this was a very fast way to get a site online without having to worry about a complex CMS.

## Why move it?

The site needed to be fully SSL so we started to look at options. [GitHub Pages][gh-pages] can run fully
[SSL](https://help.github.com/articles/securing-your-github-pages-site-with-https/) under the .io domain
but, we wanted to retain our custom domain. Also, change is good.

## Where did it move to?

We wanted to make sure we could continue to allow all staff to write new posts easily, along with
using technology to help us meet our goals. We chose to look at an option that allowed us to have
automated builds and publishing, along with redundant storage and CDN backed delivery.

We wanted builds to be done for all branches to ensure Jekyll completed, but, only deployed if the
branch was master.

We selected to use GitHub to host the Jekyll code, [Travis CI][travis-ci] to push the built
site to [S3][S3] and finally [CloudFront][CloudFront] on top with an SSL certificate. Choosing
Travis CI to do the build an deploy was something that's familiar to our team already and utilising
AWS servers gives us great flexibility moving forward.

## How we got there

One quick bit of background, all our master branches in GitHub are protected to ensure proper and full code
reviews and any required automation occurs.

At a high level, the new development and deployment process looks like this:

* Developer pulls master repo and creates a branch
* Makes changes to the site, or, writes a new post
* Developer tests locally using `bundle exec jekyll serve`
* Developer submits branch which triggers a Travis CI build (but no deploy)
* Assuming the Travis CI build passes the pull request can be reviewed and then merged
* Once merged to master, another build is done. Travis CI checks the branch name and, as it's master,
also pushes the content to [S3][S3] along with creating an invalidation at [CloudFront][CloudFront]
using [s3_website][s3_website].

The phrase, "many ways to skin a cat", is quite applicable to deploying a Jekyll site to [S3][S3] using
 [Travis CI][travis-ci] as there are certainly a few ways to handle that last furry 😸 point .

* You could use Travis' `deploy` option for s3. This is great for shipping the content but you'd then have
to install `pip` &amp; [awscli](https://pypi.python.org/pypi/awscli) so you could manually call the
[invalidation](docs.aws.amazon.com/cli/latest/reference/cloudfront/create-invalidation.html).
* You could run the build as normal, then, simply use the [script deploy](https://docs.travis-ci.com/user/deployment/script/)
to execute the `s3_website push` but thanks to how Travis CI goes back to rvm 1.9.3 the ruby version and thus the
build and [bundle is lost](https://disjoint.ca/til/2016/03/08/travis-ci-ruby-and-deployments/).
* You could use your own deploy scripts. This is simple, obvious, but requires for dependencies to be included
as part of the Travis CI setup and we wanted to try and keep that as minimal as possible.

### The detail

There's a few moving parts, but, not that many. I'm going to go in to some detail although not everything.
Hopefully however, it's enough to get you going.

Firstly you'll need a [Jekyll][jekyllrb] site.

{% highlight shell %}
$ gem install jekyll bundler
~ $ jekyll new my-awesome-site
~ $ cd my-awesome-site
~/my-awesome-site $ bundle exec jekyll serve
# => Now browse to http://localhost:4000
{% endhighlight %}

You'll also need an [S3][S3] bucket, with a static website setup and a policy. You can use [s3_website][s3_website]
 to create that, or, you can do it manually. We'll assume `s3://my-example-jekyll-site` exists. When you enable the
 static website hosting you'll have to add a bucket policy to allow access. Here's a super minimal version:

{% highlight xml %}
{
    "Version": "2008-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Principal": {
                "AWS": "*"
            },
            "Action": "s3:GetObject",
            "Resource": "arn:aws:s3:::my-example-jekyll-site/*"
        }
    ]
}
{% endhighlight %}

For our SSL certificate we used a free AWS certificate issued via their [Certificate Manager](https://aws.amazon.com/certificate-manager/).
You'll need this setup prior to going to CloudFront, or, you'll need whatever certificate/key you are going to be
using for your site.

You'll need to setup a [CloudFront][CloudFront] distribution which points to your S3 bucket website. A note here
that when you're adding the origin you can pick from the S3 buckets but change the URL to include `-website` so
it looks like this `my-example-jekyll-site.s3-website.your-aws-region.amazonaws.com`.

**Travis-CI Configuration**

Our `.travis.yml` ended up looking like this. We also post all our build notifications to a slack channel.

{% highlight yaml %}
language: ruby
cache: bundler
install: bundle install
script: bundle exec jekyll build
after_success:
  - test $TRAVIS_PULL_REQUEST == "false" && test $TRAVIS_BRANCH == "master" && bundle exec s3_website push
notifications:
  email: false
  slack:
    secure: --snip--
{% endhighlight %}

**s3_website**

Within Travis CI we've setup environment variables to take care of the S3 parts. Here's our `s3_website.yml` config.

{% highlight yaml %}
s3_id: <%= ENV['AWS_ACCESS_KEY_ID'] %>
s3_secret: <%= ENV['AWS_SECRET_ACCESS_KEY'] %>
s3_bucket: <%= ENV['S3_BUCKET_NAME'] %>
s3_endpoint: <%= ENV['AWS_DEFAULT_REGION'] %>
cloudfront_distribution_id: <%= ENV['CLOUDFRONT_DISTRIBUTION_ID'] %>
cloudfront_invalidate_root: true
cloudfront_wildcard_invalidation: true
{% endhighlight %}

Before we push this configuration to Travis CI and have it try to build it, you can (and should) test that the
credentials you've got actually work. You should get output along the lines of this:

{% highlight shell %}
export AWS_ACCESS_KEY_ID=XXXXXXXX
export AWS_SECRET_ACCESS_KEY=XXXXXXXX
export AWS_DEFAULT_REGION=your-aws-region
export S3_BUCKET_NAME=my-example-jekyll-site
export CLOUDFRONT_DISTRIBUTION_ID=XXXXXXXX

bundle exec s3_website push
[info] Downloading https://github.com/laurilehmijoki/s3_website/releases/download/v3.4.0/s3_website.jar into /home/travis/.rvm/gems/ruby-2.4.2/gems/s3_website-3.4.0/s3_website-3.4.0.jar
[info] Deploying /home/travis/build/orgname/reponame/_site/* to my-example-jekyll-site
[succ] Updated atom.xml (application/xml)
[succ] Updated rss.xml (application/rss+xml)
[succ] Updated feed.xml (application/xml)
[succ] Invalidated 1 item on CloudFront
[info] Summary: Updated 3 files. Transferred 301.6 kB, 297.0 kB/s.
[info] Successfully pushed the website to http://my-example-jekyll-site.s3-website.your-aws-region.amazonaws.com
{% endhighlight %}

We setup a specific build user in AWS with command line access and an IAM policy to allow it to do
everything it needed to and to keep it isolated from all our other IAM policies and users.

{% highlight xml %}
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
                "s3:ListBucket",
                "s3:GetBucketLocation"
            ],
            "Resource": [
                "arn:aws:s3:::my-example-jekyll-site"
            ]
        },
        {
            "Sid": "Stmt1508506753771",
            "Action": [
                "s3:PutObject",
                "s3:DeleteBucketWebsite",
                "s3:PutBucketWebsite",
                "s3:GetObject",
                "s3:DeleteObject",
                "s3:ListObjects"
            ],
            "Effect": "Allow",
            "Resource": [
                "arn:aws:s3:::my-example-jekyll-site/*",
            ]
        },
        {
            "Sid": "Stmt1508506781715",
            "Action": [
                "cloudfront:CreateInvalidation",
                "cloudfront:ListInvalidations",
                "cloudfront:GetInvalidation"
            ],
            "Effect": "Allow",
            "Resource": "*"
        }
    ]
}
{% endhighlight %}

## Other reading

There's a few posts and bits of documentation I found useful during this process which are included
here for reference:

* Travis CI - [Customizing the build](https://docs.travis-ci.com/user/customizing-the-build/)
* Travis CI - [Script Deployment](https://docs.travis-ci.com/user/deployment/script/)
* Post build deploy [error 127](https://disjoint.ca/til/2016/03/08/travis-ci-ruby-and-deployments/)

[jekyllrb]: https://jekyllrb.com/
[s3_website]: https://github.com/laurilehmijoki/s3_website
[CloudFront]: https://aws.amazon.com/cloudfront/
[S3]: https://aws.amazon.com/s3/
[gh-pages]: https://pages.github.com/
[travis-ci]: https://travis-ci.com/
