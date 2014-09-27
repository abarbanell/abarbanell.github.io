---
layout: post
title: "Moving my Blog from Wordpress to Jekyll Bootstrap"
description: ""
category: Jekyll 
tags: [Wordpress, Jekyll]
---
{% include JB/setup %}

## Rationale

I found the following points attractive which made me think about migrating to Jekyll: 

- git workflow for editing
- less infrastructure (no DB, no PHP)
- ability to serve from any static webspace, including Amazon S3
- control over plugins

## Starting point

My existing [Blog](http://abarbanell.wordpress.com) does not have so many posts, 
migration of content is not a big issue, and not a critical item 
(even if I would loose old content)

However the stats section of wrodpress.com is nice - but this can be easily replaced with 
Google Analytics.

## Setup

Here are the steps: 

- github account
- follow the quick setup from [here](http://jekyllbootstrap.com/usage/jekyll-quick-start.html), 
  it claims "from 0 to blog in 3 minutes", but this is slightly optimistic. 
  The time for github to do the first publish on a new blog is already more than 3 minutes.
- but then there were a few things not working...
  - install prerequisities like here to make sure you are up to date with all the components 
    used by github: [Installation Instructions from GitHub](https://help.github.com/articles/using-jekyll-with-pages)
  - the default markdown renderer is Kramdown, but I found that it did not render the fenced code blocks 
    (i.e. triple backquotes) correctly, so I changed it to redcarpet. Just a one line in the 
    [_config.yml](https://github.com/abarbanell/abarbanell.github.io/blob/master/_config.yml) which reads: 

```
markdown: redcarpet
```

- then of course I wanted a nicer, cleaner, more minimal theme and settled for the theme 
[Tom](https://github.com/jekyllbootstrap/theme-tom) 
and installed it with this command 

```
$ rake theme:install git="https://github.com/jekyllbootstrap/theme-tom.git"
```

## Migrating old content

I started this manual with one of my latest posts 
from [here](http://abarbanell.wordpress.com/2014/08/26/rabbitmq-on-raspberry-pi/) 
to [here](http:/linux/2014/08/26/rabbitmq-on-raspberry-pi) 
just to get some experience how to handle the different formatting, the included image assets, etc. 

Later I plan to look at migration tools.


## Themes

I will probably sooner or later fork the theme [Tom](https://github.com/jekyllbootstrap/theme-tom) 
to make some changes - this will be added in this section when I find time to do this.

 
