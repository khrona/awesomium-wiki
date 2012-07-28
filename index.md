---
layout: page
title: Welcome to the Awesomium Wiki
groups:
  - name: Getting Started
  - name: General Use
  
---
{% include JB/setup %}

Awesomium is a Web UI Bridge for Native Applications. Leverage HTML/JS/CSS to build your application's front-end.

Here you'll find tips, tricks, and tutorials for using the library in your own apps.

{% for g in site.groups %}
### {{ g.name }}
<ul>
  {% assign pages_list = site.pages %}
  {% assign group = g.name %}
  {% include JB/pages_list %}
</ul>
{% endfor %}