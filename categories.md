---
title: Categories
subtitle: Posts by category
layout: "page"
icon: fa-book
order: 3
---

{% for category in site.categories %}
  <h3>{{ category[0] }}</h3>
  <ul>
    {% for post in category[1] %}
      <li><a href="{{ post.url }}">{{ post.title }}</a></li>
    {% endfor %}
  </ul>
{% endfor %}

{%- include cat_summary.html -%}