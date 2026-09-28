---
layout: default
title: Posts
---

# Posts

<ul class="post-list">
{% for post in site.posts %}
  <li>
    <a class="post-link" href="{{ post.url | relative_url }}">{{ post.title }}</a>
    <p class="post-meta">{{ post.date | date: "%B %-d, %Y" }}</p>
    <p class="post-excerpt">{{ post.excerpt | strip_html | truncate: 140 }}</p>
  </li>
{% endfor %}
</ul>

{% if site.posts.size == 0 %}
<p>No posts yet. Add one in <code>_posts/</code>.</p>
{% endif %}
