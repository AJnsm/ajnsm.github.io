---
layout: default
title: Blog
---
<h1>Latest Posts</h1>

<ul class="bottom-2">
  {% for post in site.posts %}
    <li>
      <h2><a href="{{ post.url }}">{{ post.title }}</a></h2>
      {{ post.content | strip_html | truncatewords:40 }}
    </li>
  {% endfor %}
</ul>
{% include archive.html %}