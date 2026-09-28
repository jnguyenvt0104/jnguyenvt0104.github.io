---
layout: default
title: Posts
---

# Posts

<p class="term-line"><span class="prompt">$</span> query --all --sort=date &nbsp;// {{ site.posts.size }} entr{% if site.posts.size == 1 %}y{% else %}ies{% endif %} indexed</p>

<ul class="post-list">
{% for post in site.posts %}
  <li>
    <span class="post-index">{{ forloop.index | prepend: '00' | slice: -2, 2 }}</span>
    <div>
      <a class="post-link" href="{{ post.url | relative_url }}">{{ post.title }}</a>
      <p class="post-meta">{{ post.date | date: "%Y-%m-%d" }} · {{ post.content | number_of_words }} words</p>
    </div>
    <p class="post-excerpt">{{ post.excerpt | strip_html | truncate: 140 }}</p>
  </li>
{% endfor %}
</ul>

{% if site.posts.size == 0 %}
<p>No posts yet. Add one in <code>_posts/</code>.</p>
{% endif %}
