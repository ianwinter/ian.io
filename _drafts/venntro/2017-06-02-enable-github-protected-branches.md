---
layout: post
title: "Enable protected branches for all GitHub repositories"
author: iwinter
tags:
  - ruby
  - github
status: publish
type: post
published: true
summary: >
  A quick script to enable protected branches for all repositories in GitHub
---

**Originally posted at:** https://dev.venntro.com/2017/06/02/enable-github-protected-branches.md

We recently decided, for safety reasons and ensuring solid reviews, protecting our master branches in GitHub was something we should be doing. Having looked into it, a few quick clicks later our core repositories were done. The challenge came down the line when we needed to easily turn them all on. With >100 repositories that would be somewhat of an arduous point and click task so I knocked up a crude and basic script to do it for me.

## Reference Articles

For reference a few pages on GitHub's site are useful:

* [What are protected branches?][githubpb]
* [Create a token to use the API][newtoken]
* [API documentation for protected branches][apidocs]

## Note from API documentation

It should be noted that in the API documentation it states that protected branch API calls are in Developer Preview so you'll notice the extra part from the docs to enable this:

{% highlight bash %}
-H "Accept: application/vnd.github.loki-preview+json"
{% endhighlight %}

## The Script

{% highlight ruby %}
require "json"
require "logger"

LOGGER = Logger.new(STDOUT)
BEARER_TOKEN = ENV.fetch("BEARER_TOKEN")
ORGANIZATION = ENV.fetch("ORGANIZATION")

def run(cmd)
  LOGGER.debug("Running: #{cmd}")
  output = `#{cmd}`
  raise "Error: #{$?}" unless $?.success?
  output
end

def repos(page = 1, list = [])
  cmd = %Q{curl -s -H "Authorization: bearer #{BEARER_TOKEN}" https://api.github.com/orgs/#{ORGANIZATION}/repos?page=#{page}}
  data = JSON.parse(run(cmd))
  list.concat(data)
  repos(page + 1, list) unless data.empty?
  list
end

repos.each do |repo|
  cmd = %Q{curl -s -X PUT -H "Authorization: bearer #{BEARER_TOKEN}" -H "Accept: application/vnd.github.loki-preview+json" --data '{"required_status_checks":{"include_admins":true,"strict":true,"contexts":[]},"required_pull_request_reviews":{"include_admins":true},"restrictions":null}' https://api.github.com/repos/#{ORGANIZATION}/#{repo["name"]}/branches/master/protection}
  run(cmd)
end

{% endhighlight %}

[githubpb]: https://help.github.com/articles/about-protected-branches/
[newtoken]: https://github.com/settings/tokens/new
[apidocs]: https://developer.github.com/v3/repos/branches/#update-branch-protection

