---
layout: page
title: Music
excerpt: 'Singer, Keyboardist, Trinity Music Exams Coach'
---

<div>
  <ul>
    {% for post in site.categories['music'] %}
        {% if post.url %}
            <li><a href="{{ post.url }}">{{ post.title }}</a></li>
        {% endif %}
    {% endfor %}
    </ul>
</div>
