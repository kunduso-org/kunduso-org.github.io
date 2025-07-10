---
layout: default
title: All Posts
permalink: /posts/
---

# All Posts

<ul class="post-list">
{% for post in site.posts %}
<li class="post-item">
    <h3><a href="{{ post.url }}">{{ post.title }}</a></h3>
    <div class="post-meta">{{ post.date | date: "%B %d, %Y" }}</div>
    {% if post.tags and post.tags.size > 0 %}
    <div class="tags">
        {% for tag in post.tags %}
            <a href="/tags/#{{ tag | slugify }}" class="tag">{{ tag }}</a>
        {% endfor %}
    </div>
    {% endif %}
</li>
{% endfor %}
</ul>