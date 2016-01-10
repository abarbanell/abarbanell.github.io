# abarbanell.github.io -> http://blog.abarbanell.de

This is the git repository driving the github pages at
[blog.abarbanell.de](http://blog.abarbanell.de). The technology
used is Jekyll-Bootstrap, with my own theme.

The theme is separately available at
[abarbanell/theme-tobias](https://github.com/abarbanell/theme-tobias), it
is a fork from the well-known theme
[jekyllbootstrap/theme-tom](https://github.com/jekyllbootstrap/theme-tom).
There are graphical changes to the original theme, but also the
introduction of SASS for the css files.

Below are some parts of the original intro to Jekyll-Bootstrap modified to keep a
reminder for myself about the most relevant commands

## Jekyll-Bootstrap

The quickest way to start and publish your Jekyll powered blog. 100% compatible with GitHub pages

## Usage

For all usage and documentation please see: <http://jekyllbootstrap.com>

Some frequently used commands: 

Create a new post with: 

```
$ rake post title="Hello World"
```

## Gihtub pages with jekyll

see details [here](https://help.github.com/articles/using-jekyll-with-pages/).

start jekyll server on MacOS: 

```
$ bundle exec jekyll serve
```

## update or change theme

```
$ rake theme:install git="https://github.com/abarbanell/theme-tobias.git"
```


## License 

[MIT](http://opensource.org/licenses/MIT)
