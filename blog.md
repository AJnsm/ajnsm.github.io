---
layout: default
title: Blog
---
<h1>Latest Posts</h1>

<ul class="main-col66 bottom-2">
  {% for post in site.posts %}
    <li>
      <h2><a href="{{ post.url }}">{{ post.title }}</a></h2>
      {{ post.content | strip_html | truncatewords:40 }}
    </li>
  {% endfor %}
  {% include archive.html %}
</ul>

<!-- 

<div class="main-col33">
  <div>
    <a class="twitter-timeline"
       href="https://twitter.com/Abelaer"
       data-width="300"
       data-height="300"
       data-chrome="nofooter noscrollbar noborders transparent"
       data-tweet-limit="3">Tweets by @Abelaer</a>
    <script async src="https://platform.twitter.com/widgets.js" charset="utf-8"></script>
  </div>
</div> -->