---
title: teedoc 其它插件
keywords: teedoc, 主题插件, 主题, 插件
desc: teedoc 其它插件
---

## `teedoc-plugin-markdown-parser`: 默认 markdown 解析插件

在`site_config.json`中配置插件
```json
    "plugins": {
        "teedoc-plugin-markdown-parser":{
            "from": "pypi",
            "config": {
                "toc_depth": 3
            }
        }
    }
```

* `toc_depth`: 文章目录（右边栏）的深度， 默认 `3`， 代表到`h3` 即 `markdown` 中的`### 三级标题`

## `teedoc-plugin-jupyter-notebook-parser`: jupyter notebook 解析插件

在`site_config.json`中配置插件
```json
    "plugins": {
        "teedoc-plugin-jupyter-notebook-parser":{
            "from": "pypi",
            "config": {
            }
        }
    }
```


## `teedoc-plugin-search`: 网站搜索插件

让网站和文档支持搜索功能，支持所搜当前文档和全站搜索

要使用，在`site_config.json`中的`plugins`关键词中添加：
```json
"plugins": {
    "teedoc-plugin-search":{
        "from": "pypi",
        "config": {
            "search_hint": "Search"
        }
    }
}
```


不同文档的提示内容可以在对应的文档`config.json`中配置，以方便做多语言支持（国际化/i18n）

支持的配置如下：

```json
"teedoc-plugin-search":{
    "config": {
        "search_hint": "搜索",
        "input_hint": "输入关键词，多关键词空格隔开",
        "loading_hint": "正在加载，请稍候。。。",
        "download_err_hint": "下载文件失败，请刷新重试或检查网络",
        "other_docs_result_hint": "来自其它文档的结果",
        "curr_doc_result_hint": "当前文档搜索结果"
    }
}
```

* `search_hint`: 搜索框（按钮）的提示信息， 默认`Search`
* `input_hint`: 搜索页面搜索框输入提示信息， 默认`Keywords separated by space`
* `loading_hint`: 加载搜索所需的文件提示，默认`Loading, wait please ...`
* `download_err_hint`: 下载搜索所需的文件失败提示，需要用户刷新浏览器重试或者网络环境无法下载文件， 默认`Download error, please check network and refresh again`
* `other_docs_result_hint`: 搜索结果提示，其它文档中的搜索结果， 默认`Result from other docs`
* `curr_doc_result_hint`: 搜索结果提示，当前浏览的文档中的搜索结果， 默认`Result from current doc`


## `teedoc-plugin-baidu-tongji`: 百度统计

在每个页面添加[百度统计](https://tongji.baidu.com/)的代码，将访问信息发送到百度，就可以在后台看到访问统计信息了

在百度统计注册登录后，在管理页面添加网站，然后会有一个代码获取页面，里面会有如下的代码
```js
<script>
var _hmt = _hmt || [];
(function() {
  var hm = document.createElement("script");
  hm.src = "https://hm.baidu.com/hm.js?90c693aa2************c14a50bb49";
  var s = document.getElementsByTagName("script")[0]; 
  s.parentNode.insertBefore(hm, s);
})();
</script>
```

这里有一串密钥`90c693aa2************c14a50bb49`，将这一串密钥复制，然后在`site_config.json`中的`plugins`关键词中添加：
```json
"plugins": {
    "teedoc-plugin-baidu-tongji":{
        "from": "pypi",
        "config": {
            "code": "这里填访问密钥"
        }
    }
}
```

部署好网站后，就可以在百度统计后台实时访客页面看到访问信息了





## `teedoc-plugin-gitalk`: gitalk 评论插件

[gitalk](https://github.com/gitalk/gitalk) 是一个基于 `github` `issue`问答系统的一个评论工具， 使用 `gitalk`，无需自建服务器，只需要注册一个`github`账号， 所有的数据会放在`github`的一个仓库的`issue`上。

> 如果你遇到了问题， [这里](https://github.com/gitalk/gitalk/wiki/Q&A)也许有你要的答案

要使用， 需要在`site_config`文件中添加插件
```json
    "teedoc-plugin-gitalk": {
        "from": "pypi",
        "config": {
            "contrainer": "comments-container",
            "env": {
                "clientID": "********",
                "clientSecret": "********",
                "repo": "repo name",
                "owner": "orgnization name or user name",
                "admin": ["user names have write access"],
                "main_color": "#4caf7d",
                "second_color": "#0a7d43"
            },
        }
    }
```

* `contrainer`: 容纳评论的容器标签`id`，默认是`comments-container`, 在`teedoc-plugin-theme-default`插件里面文档页面和博客页面包含了这个标签
* `env`: `gitalk` js 插件的配置项， 更多的参数看[这里](https://github.com/gitalk/gitalk#options)。
  * `clientID`和`clientSecret`: 需要在[github application](https://github.com/settings/applications/new) 中新建一个应用，可以得到`ID`和`Secret`
  * `repo`和`owner`: 就是仓库名和拥有者，比如这里用[github.com/teedoc/teedoc.github.io](https://github.com/teedoc/teedoc.github.io)这个仓库的`issue`作为评论系统，就填`teedoc.github.io`和`teedoc`
  * `admin`: 就是拥有这个仓库写入权限的用户名
  * `main_color`: 评论主要外观颜色, 可以不设置
  * `second_color`: 评论次要外观颜色, 可以不设置

~~**每个页面开启评论需要管理员登录`gitalk`后访问页面，`gitalk`会自动创建 `issue`，并且添加标签`Gitalk`和标签`url路径`，(路径字符有`50`个字符的长度限制)**。 当然，也有批量创建的方法，这里就不介绍了，可以自行摸索。~~ 已经更新成任何人都可以创建 Issue 了

也可以先手动创建 `issue` 并且指定`issue`的标签`Gitalk`(首字母大写)，再添加另一个标签`页面的 url 路径`或者`自定义id`（标签是自定义`id`则需要在使用这个 `issue` 的页面设置页面 `id`）

`env`参数除了在`site_config`设置， 也可在文章的`metadata`中设置，在设置前面加一个`gitalk-`前缀， 比如要设置`gitalk`生成的`issue`的`title`，就可以设置`gitalk-title`。以下是几个例子：

* 如果你手动创建了一个 `issue`， 可以为某篇页面指定使用这个 `issue`, 为文章在 `metadata`中指定 `issue` 的 编号即可
```markdown
---
title: *****
gitalk-number: 2
---
```

* 或者先手动创建 `issue`， 在页面中指定使用这个带特定标签`issue`，可以看看[官方的例子](https://github.com/gitalk/gitalk/issues/1)


```markdown
---
title: *****
id: Demo
---
```

or

```markdown
---
title: *****
gitalk-id: Demo
---
```

## `teedoc-plugin-assets`: assets 资源添加插件

可以通过这个插件向页面自由添加资源， 比如`css`和`js`资源文件

比如已经添加了资源文件的路径映射，并且有`/static/css/custom.css`和`/static/css/custom.js`两个文件， 需要将其分别添加到页面头部和尾部,
同时还希望在头部添加一个`meta`标签

`site_config.json`:

```json
{
    "route": {
            "assets": {
                "/static/": "static",
            },
        },
    "plugins": {
        "teedoc-plugin-assets":{
            "from": "pypi",
            "config": {
                "header_items": [
                    "/static/css/custom.css",
                    "<meta name=\"plugin-assets\" content=\"example meta item\">"
                ],
                "footer_items": [
                    "/static/css/custom.js"
                ],
                "env":{
                    "main_color": "#000000"
                }
            }
        },
    }
}
```
如果是`css`和`js`文件，可以这样直接写`url`， 也可以写`http`开头的链接。

这里`env`里面的变量会替换到资源文件中， 在资源文件中通过`${变量名}`来使用

`custom.css`:

```css
a {
    color: ${main_color}
}
```




