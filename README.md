www.messageradius.com
=====================

Marketing Site for Message Radius


This site uses Jekyll to generate the HTML. Please see the [documentation there](http://jekyllrb.com/) for how things work together.


Blog Posts
----------

For adding a new blog post see the Jekyll guide for [Writing posts](http://jekyllrb.com/docs/posts/).

The tl;dr version goes something like this:

o create a new post, all you need to do is create a new file in the `_posts` directory. How you name files in this folder is important. Jekyll requires blog post files to be named according to the following format:

    YEAR-MONTH-DAY-title.md

Where YEAR is a four-digit number, MONTH and DAY are both two-digit numbers.

The only extra thing that this blog does that is not default in Jekyll is to add an `author` field to the YAML front matter, this is inserted into the blog post as a byline.

Out typical front matter would look like this:

```yaml
---
layout: post
title: This is the title of the post
author: Coz Worthington
---
```

