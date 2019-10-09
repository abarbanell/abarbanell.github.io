# abarbanell.github.io -> http://blog.abarbanell.de

[![Build Status](https://travis-ci.org/abarbanell/abarbanell.github.io.svg?branch=master)](https://travis-ci.org/abarbanell/abarbanell.github.io)

This is the git repository driving the github pages at
[blog.abarbanell.de](http://blog.abarbanell.de). The technology
used is Jekyll with the theme [minimal mistakes](https://mmistakes.github.io/minimal-mistakes/)

It is built by travis CI and then pushed to Amazon S3 with 
[s3_website](https://github.com/laurilehmijoki/s3_website).  

Currently no Cloudfront CDN. 

The AWS bucket is blog.abarbanell.de

The DNS is at Strato, configured to the AWS bucket at http://blog.abarbanell.de.s3-website.eu-central-1.amazonaws.com/

## HowTo
Howto run local: see [github docs](https://help.github.com/enterprise/2.9/user/articles/setting-up-your-github-pages-site-locally-with-jekyll/).

TLDR;

```
bundle exec jekyll serve --drafts
```

will bring up the site on http://localhost:4000

```
bundle update
```

will update the gemfile.lock to newest versions of all dependencies

## License 

[MIT](http://opensource.org/licenses/MIT)
