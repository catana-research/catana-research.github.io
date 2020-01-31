---
title: Tags
subtitle: Posts by tag
layout: "page"
icon: fa-book
order: 3
---

{% for tag in site.tags %}
  <h3>{{ tag[0] }}</h3>
  <ul>
    {% for post in tag[1] %}
      <li><a href="{{ post.url }}">{{ post.title }}</a> - {{ post.subtitle }} ({{ post.date | date_to_string }})</li>
    {% endfor %}
  </ul>
{% endfor %}

{%- include tag_summary.html -%}