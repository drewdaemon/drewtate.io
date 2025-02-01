---
layout: default
title: "Drew Tate's Blog"
---

<h1 class="blogTitle">Drew Tate</h1>

<ul class="postList">
  {% for post in site.posts %}
  <li>
    <span class="postDate">{{ post.date | date: "%d %b %Y" }}</span> &#10148; <a href="{{ post.url }}">{{ post.title }}</a>
  </li>
  {% endfor %}
</ul>