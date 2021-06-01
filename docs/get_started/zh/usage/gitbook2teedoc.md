---
title: 快速从 gitbook 转换为 teedoc
keywords: gitbook, teedoc
desc: 快速从 gitbook 转换为 teedoc, 摆脱 gitbook 单线程构建龟速
---


介绍如何 快速从 gitbook 转换为 teedoc, 因为 gitbook 停止了维护, 有一些特性已经不符合现在使用,比如单线程构建, 十分缓慢.

这里以[re0-web](https://github.com/lyy289065406/re0-web/tree/033f5dfd7c9d3b8654b3c1ea4dabfab8b3ebebb7)这个项目为例:


作者在 readme 中说构建一次需要 30 分钟, 但是看了一下源码, 文档数量并不是很多, 不应该需要这么长时间, 看了下作者用了 docker, 而且是在 windows 下, 自然就很慢了. 解决办法就是切换到 linux 构建, 会加快一些速度, 估计能在5分钟内构建完成.

teedoc 有着多线程构建的优势, 同时其它功能和 UI 也能满足的情况下, 可以考虑切换到 teedoc (不过 teedoc 暂时不支持 epub 和 pdf 导出(2021-05-29), 如果支持了会在[这里](../README.md)说明)

成功转换后的效果: [teedoc.github.io/re0-web-teedoc/](https://teedoc.github.io/re0-web-teedoc/)


## 新建 teedoc 工程

```
mkdir re0-web-teedoc
cd re0-web-teedoc
teedoc init
teedoc install
```

使用`teedoc serve`命令可以启动构建并起一个 http 服务, 访问`http://127.0.0.1:2333` 即可预览

使用`teedoc build`命令可以构建一个可以部署到服务器的版本, 文件在`out`目录下, 构建前可以先删除 `out` 目录


## 复制源文件


`re0-web` 这个项目的源文件在`gitbook/markdown`目录下面, 资源文件(图片)都放在`gitbook/res`, 左侧目录文件在`gitbook/SUMMARY.md`

其它文件都不重要, 都是工程文件或者生成的文件, 所以我们只需要将它们复制过来

比如创建文件夹`books/re0`, 然后将原工程 `gitbook/markdown/ch/`目录中的内容 复制到 `books/re0/ch/`目录中, 将原工程 `gitbook/markdown/jp/`目录中的内容 复制到 `books/re0/jp/`目录中, 原工程`gitbook/markdown/character.md` 和 `gitbook/README.md` 分别复制一份到`books/re0/ch/` 和 `gitbook/markdown/jp/`中

另外, 为资源文件创建一个文件夹`books/res/`, 将原工程`gitbook/res/`目录下的所有文件拷贝到`books/res/`目录下


## 为每份文档建立配置

从 `docs/get_started/zh/`目录下复制`config.json` 和 `sidebar.yaml` 文件到`books/re0/ch/`和`books/re0/jp/`

每个有`config.json` 的目录就相当于是 **一份文档**, 或者说 **一本书籍**



## 修改 site_config.json

* 修改 route

编辑`site_config.json`, 找到 `route`键值, 修改内容:
```json
     "route": {
        "docs": {
            "/gitbook/book/markdown/ch/": "books/re0/ch",
            "/gitbook/book/markdown/jp/": "books/re0/jp"
        },
        "pages": {
            "/": "pages/index/zh",
            "/en/": "pages/index/en"
        },
        "assets": {
            "/static/": "static",
            "/res/": "books/res"
        },
```

这里`url`兼容了之前的路径`gitbook/book/markdown/` 和资源路径`/res/`. 

因为`re0-web`这个项目源文件的图片都是用的绝对路径`url`比如`<img width="500" src="/res/img/article/chapter010/12.jpg" />`, 所以这里将`/res/`映射到了`books/res`文件夹;

另外源文件内可以用相对路径, 但是目录必须在同一个份文档内, 比如在本文档(点击右上角编辑本页可以看到源码), `![](../assets/images/logo.png)`: ![](../assets/images/logo.png)

因为`assets`目录也在`get_started/zh/`目录下, 所以可以直接引用, 但是超出这份文档范围的目录就不可以直接用相对路径引用了.


但是也有办法实现, 比如在这里引用`get_started/assets/`目录下的资源,在`get_started/zh/`目录下也可以引用, 只需要巧妙地配置一下 `route`, 如下:
默认
```json
    "route": {
        "docs": {
            "/get_started/zh/": "docs/get_started/zh",
        },
        "assets": {
            "/get_started/assets/": "docs/get_started/assets"
        },
```

* 修改其它

修改 `title` `domain` `source`等等


## 侧边栏目录

* 查看文档

如果这时使用`teedoc serve` 其实就可以在 `http://127.0.0.1:2333/gitbook/book/markdown/ch/character.html` 看到文档了

但是左边的目录还没有完成, 现在修改左边目录

* 转换 SUMMARY.md

进入`books/re0/ch/`, 前面我们前面复制了一个`sidebar.yaml`的文件, 现在我们需要将 gitbook 的`SUMMARY.md` 转换成这个文件格式

找到之前的`gitbook`工程的`SUMMARY.md`文件, 执行

```shell
teedoc -f 'SUMMARY.md' summary2yaml
```

我们就得到了一个`sidebar.yaml`文件, 复制到`books/re0/ch/`目录下覆盖之前的`sidebar.yaml`即可

* 修改 sidebar.yaml 的文件路径

因为之前的目录结构可能和新工程的目录结构不太像, 所以需要修改一下`sidebar.yaml`中的文件路径, 比如:

`markdown/character.md` 修改成`character.md`, 即相对与文档(有`config.json`的目录)的路径
`markdown/ch/chapter010/README.md` 修改成 `chapter010/README.md`, 可以批量替换`markdown/ch/`为空, 在很多编辑器里面都可以做到,比如`vscode`按`Ctrl+H`键来修改

如果希望链接(`url`)在新的窗口打开, 则添加一个`target: _blank`键值即可


* 查看效果

然后再执行`teedoc serve`访问`http://127.0.0.1:2333/gitbook/book/markdown/ch/index.html`就可以看到左边的目录了, 对于`jp`的目录同理可以得到


## 编辑导航栏

导航栏需要根据需要编辑一下

### 编辑模板`config/config_zh.json`

```json
{
    "class": "language_zh",
    "navbar": {
        "title": "re0-web",
        "logo": {
            "alt": "re0-web logo",
            "src": ""
        },
        "home_url": "/",
        "items": [
            {
                "url": "/gitbook/book/markdown/ch/",
                "label": "中文",
                "position": "left"
            },
            {
                "url": "/gitbook/book/markdown/jp/",
                "label": "日本语",
                "position": "left"
            },
            {
                "url": "https://github.com/teedoc/re0-web-teedoc",
                "label": "github",
                "target": "_blank",
                "position": "right"
            },
            {
                "id": "language",
                "label": "Language: ",
                "position": "right",
                "items": [
                    {
                        "id": "zh",
                        "url": "/",
                        "label": "中文"
                    },
                    {
                        "id": "en",
                        "url": "/jp/",
                        "label": "日本语"
                    }
                ]
            }
        ]
    },
    "footer":{
        "top":[
            {
                "label": "链接",
                "items": [
                    {
                        "label": "使用 teedoc 构建",
                        "url": "https://teedoc.neucrack.com",
                        "target": "_blank"
                    },
                    {
                        "label": "网站地图",
                        "url": "/sitemap.xml"
                    }
                ]
            },
            {
                "label": "源码",
                "items": [
                    {
                        "label": "本网站源文件",
                        "url": "https://github.com/teedoc/re-web-teedoc",
                        "target": "_blank"
                    }
                ]
            }
        ],
        "bottom": [
        ]
    },
    "plugins": {
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
    },
    "show_source": "编辑本文"
}
```

另外一个 `config_en.json` 可以更名为 `config_jp.json`, 同理修改下内容, 顺便重命名`pages/index/en`为`pages/index/jp`

```json
{
    "navbar": {
        "title": "re0-web",
        "logo": {
            "alt": "re0-web logo",
            "src": ""
        },
        "home_url": "/jp/",
        "items": [
            {
                "url": "/gitbook/book/markdown/ch/",
                "label": "中文",
                "position": "left"
            },
            {
                "url": "/gitbook/book/markdown/jp/",
                "label": "日本语",
                "position": "left"
            },
            {
                "url": "https://github.com/neutree/re0-web-teedoc",
                "label": "github",
                "target": "_blank",
                "position": "right"
            },
            {
                "id": "language",
                "label": "语言: ",
                "position": "right",
                "items": [
                    {
                        "id": "zh",
                        "url": "/",
                        "label": "中文"
                    },
                    {
                        "id": "en",
                        "url": "/jp/",
                        "label": "日本语"
                    }
                ]
            }
        ]
    },
    "footer":{
        "top":[
            {
                "label": "Links",
                "items": [
                    {
                        "label": "Site generated by teedoc",
                        "url": "https://teedoc.neucrack.com",
                        "target": "_blank"
                    },
                    {
                        "label": "Sitemap",
                        "url": "/sitemap.xml"
                    }
                ]
            },
            {
                "label": "Source code",
                "items": [
                    {
                        "label": "website source files",
                        "url": "https://github.com/teedoc/re0-web-teedoc",
                        "target": "_blank"
                    }
                ]
            }
        ],
        "bottom": [
        ]
    }
}
```

### 修改每本书籍(/文档)的配置文件 config.json

* 修改 `books/re0/ch/config.json`

```json
{
    "import": "config_zh",
    "navbar": {
        "items": [
            {
                "id": "language",
                "label": "Language: ",
                "position": "right",
                "items": [
                    {
                        "id": "zh",
                        "url": "/gitbook/book/markdown/ch/",
                        "label": "中文"
                    },
                    {
                        "id": "en",
                        "url": "/gitbook/book/markdown/jp/",
                        "label": "日本语"
                    }
                ]
            }
        ]
    }
}
```

* 修改 `books/re0/jp/config.json`

```json
{
    "import": "config_jp",
    "navbar": {
        "items": [
            {
                "id": "language",
                "label": "Language: ",
                "position": "right",
                "items": [
                    {
                        "id": "zh",
                        "url": "/gitbook/book/markdown/ch/",
                        "label": "中文"
                    },
                    {
                        "id": "en",
                        "url": "/gitbook/book/markdown/jp/",
                        "label": "日本语"
                    }
                ]
            }
        ]
    }
}
```

## 编辑首页

编辑`pages/index/zh/README.md`, 根据需要修改即可


## 部署

和 `gitbook` 一样, `teedoc build`生成的都是静态文件,并且放在 `out`文件夹下, 直接拷贝到服务器部署即可,更多请看[文档](./deploy.md)

最终效果: [teedoc.github.io/re0-web-teedoc/](https://teedoc.github.io/re0-web-teedoc/)
