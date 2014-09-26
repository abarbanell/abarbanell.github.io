---
layout: page
title: Tobias Abarbanell
tagline: Notes on Software Development
---
{% include JB/setup %}

Here is the list of posts in this blog: 

<ul class="posts">
  {% for post in site.posts %}
    <li><span>{{ post.date | date_to_string }}</span> &raquo; <a href="{{ BASE_PATH }}{{ post.url }}">{{ post.title }}</a></li>
  {% endfor %}
</ul>

## To-Do

- add About page
- add posts
- change theme to something nicer

