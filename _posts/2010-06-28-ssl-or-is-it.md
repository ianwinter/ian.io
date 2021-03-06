---
layout: post
title: SSL, or is it?
tags:
- apache
- coldfusion
- rails
- ruby
- ssl
status: publish
type: post
canonical_url: https://dev.venntro.com/2010/06/ssl-or-is-it/
published: true
meta:
  _edit_last: "4"
  _pingme: "1"
  _encloseme: "1"
  _wp_old_slug: ""
  _syntaxhighlighter_encoded: "1"
---
<p>We manage a number of sites with secure content. Installing SSL certificates and keys on multiple servers becomes a time consuming annoyance.</p>

<p>To ease this pain we've made use of a SSL decryption module added into our load balancers. Using the module means we can install the SSL certificate on the load balancer and it decrypts SSL traffic and then passes around internally using standard HTTP. This reduces the decryption load on the individual web servers leaving the dedicated module on the load balancer to handle all SSL.</p>

<p>This does however raise the problem that if internal servers see just HTTP traffic how do they know some traffic should be secure? This is important so that our application can work securely for areas of the site like credit card payment. We could check ports, but checking ports isn't ideal. The answer is simple. All you need is a request header set in your Apache config and you're away:</p>

<p>For our Ruby on Rails applications:</p>

{% highlight conf %}
RequestHeader set X_FORWARDED_PROTO "https"
{% endhighlight %}

<p>For our ColdFusion applications:</p>

{% highlight conf %}
RequestHeader set HTTPS "on"
{% endhighlight %}

<p>This means with simple header checks we can switch the behaviour of the application easily.</p>

<em>Orginally published at <a href="{{ page.canonical_url }}">dev.venntro.com</a></em>
