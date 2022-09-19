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
                "parse_files": ["md"],
                "mermaid": true,
                "mermaid_use_cdn": false,
                "mermaid_cdn_url": "https://cdn.jsdelivr.net/npm/mermaid/dist/mermaid.min.js",
                "mathjax": {
                    "enable": true,
                    "file_name": "tex-mml-chtml",
                    "config": {
                        "loader": {
                            "load": ["output/svg"]
                        },
                        "tex": {
                            "inlineMath": [["$", "$"], ["\\(", "\\)"]]
                        },
                        "svg": {
                            "fontCache": "global"
                        }
                    }
                }
            }
        }
    }
```

* `parse_files`: 参与解析的文件格式
* `mermaid`: 是否开启 mermaid 插件
* `mermaid_use_cdn`: 是否使用 CDN 加载 mermaid 插件
* `mermaid_cdn_url`: CDN 地址, `mermaid_use_cdn` 为 `true` 时才有用
* `mathjax`: 数学公式渲染支持
  * `enable`:  是否启用
  * `file_name`： `js`文件名， 定义了默认有哪些功能，默认`tex-mml-chtml`, [参考这里](http://docs.mathjax.org/en/latest/web/components/index.html)
  * `config`: `mathjax`的配置， [mathjax](https://www.mathjax.org/)的配置， 具体配置项看[这里](http://docs.mathjax.org/en/latest/web/configuration.html)


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
            "search_hint": "Search",
            "env": {
                "main_color": "#4caf7d",
                "main_color_dark": "#1b4c33",
                "hint_shadow_color": "rgba(76, 175, 125, 0.38)"
            }
        }
    }
}
```


不同文档的提示内容可以在对应的文档`config.json`中配置，以方便做多语言支持（国际化/i18n）, 当然，插件默认支持了`中文``英文`和`日文`， 翻译在[这里](https://github.com/teedoc/teedoc/tree/main/plugins/teedoc-plugin-search/teedoc_plugin_search/locales)， 当默认找不到会使用`英文`， 手动设置下面这边变量会覆盖插件自带的翻译

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
* `env`: 替换部分颜色， 包括
  * `main_color`: 主颜色， 默认`"#4caf7d"`
  * `main_color_dark`: 黑色主题主颜色， 默认`"#1b4c33"`
  * `hint_shadow_color`: 阴影颜色，默认`rgba(76, 175, 125, 0.38)`

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


## `teedoc-plugin-google-analytics`: Google 分析

在每个页面添加[Google 分析](https://analytics.google.com/)的代码，将访问信息发送到 Google，就可以在后台看到访问统计信息了

在 Google 分析 注册登录后，在管理页面添加网站，然后会有一个 `ID`，以`UA-`或者`G-`开头， 比如`UA-123456789-1` `G-AABBCCDDJJOOKK`

然后在`site_config.json`中的`plugins`关键词中添加：
```json
"plugins": {
    "teedoc-plugin-google-analytics":{
        "from": "pypi",
        "config": {
            "id": "这里填资源 ID"
        }
    }
}
```


## `teedoc-plugin-comments-gitalk`: gitalk 评论插件

[gitalk](https://github.com/gitalk/gitalk) 是一个基于 `github` `issue`问答系统的一个评论工具， 使用 `gitalk`，无需自建服务器，只需要注册一个`github`账号， 所有的数据会放在`github`的一个仓库的`issue`上。

> 如果你遇到了问题， [这里](https://github.com/gitalk/gitalk/wiki/Q&A)也许有你要的答案


### 配置 github

首先到 [github 开发者设置](https://github.com/settings/developers)里面,
* 添加 一个 Oauth Apps
* 然后生成 secrets， 就获得了 client ID 和 secrets
* 填写 `Homepage URL` 为 主页 url，比如`https://teedoc.github.io`
* 填写 `Authorization callback URL`为首页`https://teedoc.github.io/`


### 配置文档



要使用， 需要在`site_config`文件中添加插件
```json
    "teedoc-plugin-comments-gitalk": {
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
  * `adminAutoCreate`: 是否在管理员访问页面的时候就立刻创建 issue
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


## `teedoc-plugin-google-translate`: Google 页面翻译插件

功能和 `Chrome` 的鼠标右键翻译此页面 功能一样

翻译前：

![google translate](../../assets/images/google_translate0.png)

翻译后：

![google translate](../../assets/images/google_translate.png)



```json
"config": {
    "lang": "auto",
    "doc_types": ["page", "doc", "blog"],
    "domain": "/"
}
```

* `lang`: 需要被翻译的页面的语言， 默认**建议不要设置**， 或者设置为文档的`locale`一致， 比如`locale`为`zh_CN`，则这个可以设置为`zh-CN`
* `doc_types`: 要展示在那些类型的文档页面中，`["page", "doc", "blog"]` 中的 0 个 到 3 个，
* `domain`: 从哪个网址下载`google translate`的源码，默认是`"/"`, 代表从本网站下载，也可以设置成 `translate.google.com` 或者 `translate.google.cn`
> 主要是考虑到中国地区不开代理无法下载请求的某些文件，实际还是要请求`translate.googleapis.com`的， 在制作插件时发现就算设置成`translate.google.cn`，有个`js`文件请求了`google.com`的域名下的一个`cleardot.gif`文件，导致网络请求卡住很久直到超时才能加载翻译，所以把那个`js`文件（`element_main.js`）修改并放到了本地，这样就可以很快地访问了。
> 但是这样也有风险，由于代码和 `Google`服务器的代码分离了，万一哪天`Google`更新了代码，我们也需要跟着手动更新（虽然大概率不会改），到时欢迎更新修改并提交 PR


## `teedoc-plugin-ad-hint`: 广告或者重要消息全局提示

### 效果

![hint](../../assets/images/hint.jpg)

### 使用

在`site_config`中添加插件

```json
"plugins": {
    "teedoc-plugin-ad-hint": {
        "from": "pypi",
        "config": {
            "type": "hint",
            "label": "☆",
            "content": "内容，支持 html 语法，不要使用英文单引号",
            "show_after_s": 432000
        }
    }
}
```

配置：默认配置代码在[这里](https://github.com/teedoc/teedoc/blob/e909be61546e3817b872bd5005b9b867c6843e60/plugins/teedoc-plugin-ad-hint/teedoc_plugin_ad_hint/__init__.py#L22)

* `type`: 默认 `"hint"`, 目前只支持`hint`
* `label`: 默认 `"New"`, 会自动在导航栏添加一个标签，点击会显示顶部提示内容，这里设置标签显示的内容
* `content`: 默认 `""`, 内容，`hint`类别的内容会显示在顶部
* `show_times`: 默认 `2`,  设置用户访问多少次页面后提示不在自动显示，如果值 `<= 0` 则永远显示
* `show_after_s`: 默认 `432000`,  设置下次再自动显示的时间，单位是秒，默认是 `5` 天
* `date`: 默认值为`null`, 提示信息的更新时间，如果更新了信息，可以设置这个时间来让提示立即自动显示，格式:`2021-11-07 14:23:00`, 如果设置的时间大于当前实际的时间，则会一直保持自动显示
* `color`: 默认 `"#a0421d"`, 字体颜色
* `link_color`: 默认 `"#e53935"`, 链接（`<a>`标签）字体颜色
* `link_bg_color`: 默认 `"#e6ae5c"`, 链接（`<a>`标签）背景颜色
* `bg_color`: 默认 `"#ffcf89"`, 背景颜色
* `color_hover`: 默认 `"white"`, 鼠标悬浮时的字体颜色
* `bg_color_hover`: 默认 `"#f57c00"`, 鼠标悬浮时的背景颜色
* `close_color`: 默认 `"#eab971"`， 关闭按钮背景颜色

### 国际化

如果文档有多语言，可以在`site_config`中配置插件:
```json
"plugins": {
    "teedoc-plugin-ad-hint": {
        "from": "pypi",
        "config": {
            "type": "hint",
            "label": "☆",
            "content": "content",
            "show_after_s": 432000
        }
    }
}
```

然后在不同语言的文档`config`中覆盖设置
```json
"plugins": {
    "teedoc-plugin-ad-hint": {
        "config": {
            "label": "🦀",
            "content": "内容，支持 html 语法，不要使用英文单引号",
        }
    }
}
```

同一个语言的文档尽量使用同一份`config`模板文件, 在具体的文档`config`中`import`，更容易配置

## `teedoc-plugin-thumbs-up`: 点赞插件

用这个插件可以实现页面显示点赞按钮，可以统计点赞次数，效果如下
![thumbs_up.png](../../assets/images/thumbs_up.png)

### 基本使用

在`site_config`中添加插件：
```json
"plugins": {
    "teedoc-plugin-thumbs-up": {
        "from": "pypi",
        "config": {
            "url": "https://xxxx.xxxx.com:80",
            "show_up_count": true,
            "show_down_count": false
        }
    }
}
```

需要注意的是，因为网页是纯静态页面，使用这个插件需要配合服务器使用，统计信息储存在服务器上。

服务器使用`pip install teedoc-plugin-thumbs-up`命令安装插件后使用`teedoc-thumbs-up-server`命令来运行服务，可以用`--port`参数来指定端口，更多参数使用`teedoc-thumbs-up-server --help`查看
> 建议用 nginx 等代理服务器代理这个服务。
> 另外如果你的网站是 https 协议，那这个服务也需要是 https 服务，可以申请免费的证书后用 nginx 配置

需要注意的是，这个服务只是个简单的例程，不完全保证服务安全性，建议仔细阅读这个服务器代码修改定制你自己的服务器脚本，代码：[teedoc-plugin-thumbs-up/server](https://github.com/teedoc/teedoc/tree/main/plugins/teedoc-plugin-thumbs-up/server)

### 国际化

和其它插件一样，在不同语言的文档`config`中覆盖设置，参数见[插件源码默认配置](https://github.com/teedoc/teedoc/blob/a8b93a4f29330b018c2e38589786b33d00854309/plugins/teedoc-plugin-thumbs-up/teedoc_plugin_thumbs_up/__init__.py#L24)
```json
"plugins": {
    "teedoc-plugin-thumbs-up": {
        "config": {
            "label_up": "有帮助",
            "label_down": "没帮助",
            "msg_already_voted": "您已经投过票了",
            "msg_thanks": "感谢您的反馈",
            "msg_down_prompt": "感谢反馈，请告诉我们可以改进什么地方?（最少 10 个字）",
            "msg_down_prompt_error": "消息最少需要 10 个字， 最多 256 个字",
            "msg_error": "请求服务器出现错误!"
        }
    }
}
```




