---
layout: page
title: Code Snippets
permalink: /code-snippets/
---

This will be a collection of code snippets that I've got littered about and want to share and make searchable. The couple that are here are just place holders, I'll start adding them in when I can.

# Current Categories

{% assign categories = site.code-snippets | where:"parent","true" %}

{% for category in categories %}

  {% assign catslug = category.title | downcase %}
  {% assign catcount = site.code-snippets | where:"language",catslug | where:"parent","false" %}

  * [{{ category.title }}](/code-snippets/{{ category.title|downcase }}/ "{{ category.title }}") ({{catcount|size }}) {{ relative_path }}

{% endfor %}

