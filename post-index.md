---
title: Post index
layout: "page"
icon: fa-book
order: 3
published: False
---

<ul>
  {% for post in site.posts %}
    <li>
      <a href="{{ post.url }}">{{ post.title }}</a>
    </li>
  {% endfor %}
</ul>