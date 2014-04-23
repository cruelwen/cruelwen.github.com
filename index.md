---
layout: page
title: Welcome to Cruel World!
tagline: Supporting tagline
---
{% include JB/setup %}

<ul>
  {% for post in site.posts %}
  <li>
    <h4><a href="{{ post.url }}">{{ post.title }}</a></h4>
  </li>
  {% endfor %}
</ul>
