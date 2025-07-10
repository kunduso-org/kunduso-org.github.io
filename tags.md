---
layout: default
title: Tags
permalink: /tags/
---

# Tags

{% assign tags = site.tags | sort %}
{% for tag in tags %}
  {% assign tag_name = tag[0] %}
  {% assign tag_posts = tag[1] %}
  <h3 id="{{ tag_name | slugify }}">{{ tag_name }} ({{ tag_posts.size }})</h3>
  <ul class="post-list">
    {% for post in tag_posts %}
    <li class="post-item">
        <h4><a href="{{ post.url }}">{{ post.title }}</a></h4>
        <div class="post-meta">{{ post.date | date: "%B %d, %Y" }}</div>
    </li>
    {% endfor %}
  </ul>
{% endfor %}