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

- [ ] add About page
- [ ] add posts
- [x] change theme to something nicer
- [ ] improve markdown
  - [x] code blocks
  - [ ] github style checkboxes
  - [ ] references to github issues (may only apply for project posts?)


## Sample code

this is an example of a shell script

```
echo a code block is a block which is indented
echo there should be line breaks between the "echo" statements
echo this is the last line
```

and here we go with normal text, folowed by a task list with checkboxes: 

- [ ] not done, first level
  - [x] done, second level
  - [x] done, second level, task #1234
  - [ ] not done, second level
- [x] done, first level

