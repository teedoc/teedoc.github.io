---
title: 使用 teedoc 写博客
keywords: teedoc, 博客, 静态博客, 博客生成, blog
desc: teedoc 写博客（生成博客）
---

## 使能博客插件，并配置路径映射

在`site_config.json`中的`route`关键字中，添加`blog`关键字，并且添加`teedoc-plugin-blog`插件
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

比如这里`/blog/`就是生成的博客网页路径（url），`blog`就是源文件的路径，和文档和资源文件的`route`设置类似

然后执行安装插件的命令
```shell
teedoc install
```


## 创建博客文件夹

* 在刚刚配置的`blog`文件夹中建立一个`config.json`，使用方法同前面说的文档的`config.json`相同。
* 新建一个`README.md`文件，用来写博客的首页，比如只写头信息，不写内容，生成的网页会在这个文件渲染后的基础上添加博客列表，这里`date: false` 代表不在这个页面显示日期

```markdown
---

title: 博客
keywords: teedoc, 博客生成, 静态博客
desc: teedoc 静态博客页面生成
show_source: false
date: false

---

```

## 创建博客文件

在`blog`目录下的任意位置，创建博客文件，格式为`md`，写入内容即可。同样，可以添加头信息，比如
```markdown

---

title: teedoc 博客支持
keywords: teedoc, 博客, 生成博客, 静态博客
desc: teedoc 生成静态博客页面
author: neucrack
date: 2021-03-14
tags: hello, blog, teedoc
cover: ./assets/cover.jpg

---

这里也可以使用图片![图片](./assets/img.jpg)
这是一段测试简介，会显示在列表中，使用`<!-- more -->`来分隔正文

<!-- more -->


## 正文标题一

正文


```

>! 注意 `tags` 和 `keywords` 是以英文逗号隔开不是中文逗号


这里用了`<!-- more -->`来分割了文章，前部分会被作为简介显示到博客首页列表中，方便读者预览，如果没有分割，则会取前`500`个字符作为简介

## 生成页面

执行`teedoc serve`命令来起一个服务即可访问`http://127.0.0.1/blog/`， 也可以将`/blog/`这个链接在`config.json`中加入到导航栏，作为博客入口。
比如：
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
                "label": "首页",
                "position": "left"
            },
            {
                "url": "/get_started/zh/",
                "label": "安装使用",
                "position": "left"
            },
            {
                "url": "/develop/zh/",
                "label": "开发",
                "position": "left"
            },
            {
                "url": "/blog/",
                "label": "博客",
                "position": "left"
            }
    }
}
```

