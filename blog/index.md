---
layout: page
title: Blog
excerpt: A List of blog posts by Balaji Soundararajan
---

<div>
  <ul>
    {% for post in site.categories['blog'] %}
        {% if post.url %}
            <li><a href="{{ post.url }}">{{ post.title }}</a></li><br>
        {% endif %}
    {% endfor %}
    </ul>
</div>
