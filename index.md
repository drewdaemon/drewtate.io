---
layout: default
title: "Drew Tate's Blog"
---

<h1 class="blogTitle">Drew Tate</h1>

{% assign pinned_posts = site.posts | where: "pinned", true %}
{% assign regular_posts = site.posts | where_exp: "post", "post.pinned != true" %}

{% if pinned_posts.size > 0 %}

<ul class="postList">
  {% for post in pinned_posts %}
  <li>
    <span class="postDate">Evergreen 🌲</span> &#10148; <a href="{{ post.url }}">{{ post.title }}</a>
  </li>
  {% endfor %}
</ul>

<hr>

{% endif %}

<ul class="postList">
  {% for post in regular_posts %}
  <li>
    <span class="postDate">{{ post.date | date: "%d %b %Y" }}</span> &#10148; <a href="{{ post.url }}">{{ post.title }}</a>
  </li>
  {% endfor %}
</ul>
