---
layout: page
title: Tech
excerpt: Tech Blogs by Balaji Soundararajan
---

<div>
  <ul>
    {% for post in site.categories['tech'] %}
        {% if post.url %}
            <li><a href="{{ post.url }}">{{ post.title }}</a></li><br>
        {% endif %}
    {% endfor %}
    </ul>
</div>
