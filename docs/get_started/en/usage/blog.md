---
title: Use teedoc to write a blog
keywords: teedoc, blog, static blog, blog generation, blog
desc: teedoc blogging (generating blog)
---

## Enable blog plugin and configure path mapping

In the `route` keyword in `site_config.json`, add the `blog` keyword and add the `teedoc-plugin-blog` plugin
```json
    "route": {
        "blog": {
            "/blog/": "blog"
        }
    },
    "plugins": {
            "teedoc-plugin-blog": {
            "from": "pypi",
            "config": {
            }
        }
    }
```

For example, here `/blog/` is the path (url) of the generated blog webpage, and `blog` is the path of the source file, similar to the `route` setting of documents and resource files

Then execute the command to install the plugin
```shell
teedoc install
```


## Create a blog folder

* Create a `config.json` in the `blog` folder you just configured, and use the same method as the `config.json` of the aforementioned document.
* Create a new `README.md` file to write the homepage of the blog. For example, only the header information is written, but the content is not written. The generated web page will add the blog list after the file is rendered, where `date: false` represents Do not display the date on this page

```markdown
---

title: Blog
keywords: teedoc, blog generation, static blog
desc: teedoc static blog page generation
show_source: false
date: false

---

```

## Create a blog file

Create a blog file at any location under the `blog` directory, in the format of `md`, and just write the content. Similarly, you can add header information, such as
```markdown

---

title: teedoc blog support
keywords: teedoc, blog, generated blog, static blog
desc: teedoc generates static blog pages
author: neucrack
date: 2021-03-14
tags: hello, blog, teedoc
cover: ./assets/cover.jpg

---

Also can use image: ![image](./assets/img.jpg)

This is an introduction to the test, which will be displayed in the list. Use `<!-- more -->` to separate the text

<!-- more -->


## Body Title One

text


```

>! Note that `tags` and `keywords` are separated by English commas, not Chinese commas


Here, `<!-- more -->` is used to divide the article. The first part will be displayed as an introduction to the blog homepage list, which is convenient for readers to preview. If there is no division, the first `500` characters will be taken as the introduction.

## Generate page

Execute the `teedoc serve` command to start a service to access `http://127.0.0.1/blog/`, or add the link `/blog/` to the navigation bar in `config.json` as a blog Entrance.
such as:
```json
{
    "navbar": {
        "title": "teedoc",
        "logo": {
            "alt": "teedoc logo",
            "src": "/static/image/logo.png"
        },
        "home_url": "/",
        "items": [
            {
                "url": "/",
                "label": "Home",
                "position": "left"
            },
            {
                "url": "/get_started/zh/",
                "label": "Installation and Use",
                "position": "left"
            },
            {
                "url": "/develop/zh/",
                "label": "Development",
                "position": "left"
            },
            {
                "url": "/blog/",
                "label": "blog",
                "position": "left"
            }
    }
}
```

