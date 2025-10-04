---
layout: default
title: Blog
---
<h1>Latest Posts</h1>

<ul>
  {% for post in site.posts %}
    <li>
      <a href="{{ post.url | prepend:"" }}">{{ post.title }}</a>
    </li>
  {% endfor %}
</ul>
