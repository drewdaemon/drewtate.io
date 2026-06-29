---
layout: default
title: "Drew Tate's Blog"
---

<h1 class="blogTitle">Drew Tate</h1>

{% assign all_tags = site.posts | map: "tags" | compact | flatten | uniq | sort %}
<p class="tagList">
  Tags:
  {% for tag in all_tags %}
    <a href="/tags/{{ tag | slugify }}/">{{ tag }}</a>{% unless forloop.last %}, {% endunless %}
  {% endfor %}
</p>

<ul class="postList">
  {% for post in site.posts %}
  <li>
    <span class="postDate">{{ post.date | date: "%d %b %Y" }}</span> &#10148; <a href="{{ post.url }}">{{ post.title }}</a>
    {% if post.tags %}
      <span class="postTags">
        {% for tag in post.tags %}
          <a href="/tags/{{ tag | slugify }}/">{{ tag }}</a>{% unless forloop.last %}, {% endunless %}
        {% endfor %}
      </span>
    {% endif %}
  </li>
  {% endfor %}
</ul>