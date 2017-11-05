---
layout: post
title: "Adding HTTP headers with Lambda@Edge"
author: iwinter
tags:
  - cloudfront
  - lambda
status: publish
type: post
published: true
canonical_url: https://dev.venntro.com/2017/10/adding-http-headers-with-lambda-edge/
summary: >
  A brief overview of how we used Lambda@Edge to add custom HTTP headers to our static
  site hosted with CloudFront and S3.
---
First of all, what is [Lambda@Edge][lae]? The best description comes from Amazon themselves:

> With Lambda@Edge, you can easily run your code across AWS locations globally, allowing you to respond to your end users
at the lowest latency. Your code can be triggered by Amazon CloudFront events such as requests for content to or from origin
servers and viewers. Upload your Node.js code to AWS Lambda and Lambda takes care of everything required to replicate, route
and scale your code with high availability at an AWS location close to your end user. You only pay for the compute time you
consume - there is no charge when your code is not running.

[Lambda](https://aws.amazon.com/lambda/) is Amazon's offering in the what's often referred to as
"[serverless](https://aws.amazon.com/serverless/)" computing space.

In my [last post](/2017/10/26/automating-the-build-and-deployment-of-our-team-site-with-jekyll-github-travis-s3-and-cloudfront.html),
I talked about how we automated the build and deployment of our team site with Jekyll, GitHub, Travis, S3 and CloudFront.

One of the important elements of hosting any site nowadays are  HTTP security headers. There are a number of resources
available about these and the importance of them so I'm not going to go in to much detail but I will provide some
links to a few of the more useful tools and articles I've read.

* [Everything you need to know about HTTP security headers](https://blog.appcanary.com/2017/http-security-headers.html)
* [Hardening your HTTP response headers](https://scotthelme.co.uk/hardening-your-http-response-headers/) - Scott Helme
* [Hardening Your HTTP Security Headers](https://www.keycdn.com/blog/http-security-headers/) - MaxCDN
* [securityheaders.io](https://securityheaders.io/) - a tool to check headers

With an S3 website, you can control cache headers by adding information to the object metadata but controlling HTTP headers
isn't possible. If you were hosting your site with something like [nginx](http://nginx.org), it'd be easy to simply edit your
 `server` block and set some headers. Something like this:

{% highlight nginx %}
add_header     X-Content-Type-Options "nosniff";
add_header     X-Frame-Options "DENY";
add_header     X-XSS-Protection "1; mode=block";
add_header     Referrer-Policy "same-origin";
{% endhighlight %}

Last year (2016), [Jeff Barr](https://twitter.com/jeffbarr) announced a [preview](https://aws.amazon.com/blogs/aws/coming-soon-lambda-at-the-edge/)
of [Lambda@Edge][lae] to deal with this and in July 2017 that became generally available and
[Jeff posted](https://aws.amazon.com/blogs/aws/lambdaedge-intelligent-processing-of-http-requests-at-the-edge/)
how it's now possible to have intelligent processing of HTTP requests at the edge.

I'd seen an article previously about how to use the preview to add headers, but, as the author notes this is no
longer valid because the format of functions within [Lambda@Edge][lae] changed. After some searching I found
[this article](https://nvisium.com/blog/2017/08/10/lambda-edge-cloudfront-custom-headers/) from nvisium.com which had
an excellent overview and guide on how to set it all up.

You'll need to create a IAM role to let [Lambda@Edge][lae] talk to [CloudFront][] but there are templates as part of the
[Lambda@Edge][lae] console to help you do this.

Once I'd created the Lambda function, added the headers I wanted and saved the version I could then reference it in our
[CloudFront][] distributions. Note that I chose to add the function editing the [CloudFront][] distribution rather than letting
Lambda do it for me. You'll also always need to include the version after the ARN, so, something like `arn:aws:lambda:us-east-1:123456789000:function:functionName:VERSION`

If you curl our team site URL you'll see the custom headers (note, some removed from this snippet):

{% highlight shell %}
$ curl -I https://dev.venntro.com
HTTP/1.1 200 OK
Date: Mon, 30 Oct 2017 11:24:57 GMT
Last-Modified: Fri, 27 Oct 2017 08:40:55 GMT
Referrer-Policy: no-referrer-when-downgrade
Server: AmazonS3
x-amz-id-2: ...
x-amz-request-id: ...
X-Content-Type-Options: nosniff
X-Frame-Options: DENY
X-XSS-Protection: 1; mode=block
X-Cache: Hit from cloudfront
X-Amz-Cf-Id: ...
{% endhighlight %}

**Conclusion and notes**

[Lambda@Edge](lae) is only currently available in us-east-1 (console/GUI) although it can be used in other regions.

I'd like to note that Lambda and Lambda@Edge are both still young services and as such are developing rapidly. One
issue I noticed which has been discussed online a lot is that replicated Lambda functions cannot be deleted.
What this means is your console can get filled rapidly with old or testing versions. Hopefully this will be fixed in
the future.

We do however now have HTTP headers being served across all our [CloudFront][] backed sites using [Lambda@Edge][lae]
ensuring we're keeping our sites following best practices.

<em>Orginally published at <a href="{{ page.original }}">dev.venntro.com</a></em>

[lae]: https://aws.amazon.com/lambda/edge/
[CloudFront]: https://aws.amazon.com/cloudfront/
