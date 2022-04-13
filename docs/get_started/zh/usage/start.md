---
title: 文档结构和配置
keywords: teedoc, 目录结构, teedoc 文档配置, teedoc 文档结构, teeodc sidebar
desc: teedoc， 将 markdown 或者 jupyter notbook 转换成 html 静态网页， 介绍了 teedoc 的基本使用， 包括文档结构和配置
---


## 构建与预览

在有`site_config.json`的文档目录下执行
```shell
teedoc serve
```

在显示 `Starting server at 0.0.0.0:2333 ....` 后，就可以了

打开浏览器访问: [http://127.0.0.1:2333](http://127.0.0.1:2333)


实时修改文件，保存文件后，默认过`3`秒后，会自动重新构建这个文件，然后浏览器会自动刷新
> 自动刷新的延迟时间可以设置，可以加 `-t` 参数， 比如`teedoc -t 0 serve`设置为`0`秒延迟，
> 另外也可以在文档配置中设置，见后面配置参数`rebuild_changes_delay`的说明


如果只需要构建生成`HTML`页面，只需要执行

```shell
teedoc build
```

>! 注意，如果是最终生成发布版本的文档， 一定要用`build`命令来生成网站页面，`serve`命令生成的页面只能用于本地预览，会有多余的预览相关的代码，不适合用在生产环境部署


另外，也可以指定参数`-d`或者`--dir` 来指定文档目录，这样就不用在文档目录下面执行命令了，比如
```shell
teedoc -d /home/teedoc/my_doc build
```


## 构建文档删除


构建好的文档会被放到`out`目录下，程序不会主动删除，如果需要清除，请手动删除



## 文档目录结构

```shell
├─.github
├─docs
│  ├─develop
│  │  ├─en
│  │  └─zh
│  └─get_started
│      ├─assets
│      ├─en
│      └─zh
├─pages
│  └─index
│      ├─en
│      └─zh
├─static
│
└─site_config.json
```

* `.github`: 自动构建脚本，在后面的章节将如何使用
* `docs`: 文档，包含了多份文档， 每份文档都是单独一个文件夹
* `pages`: 页面，包括主页、404页面等等页面
* `static`: 静态文件文件夹，比如存放图片
* `site_config.json`: 网站配置文件
* `config.json`: 除了`site_config.json`外，每个文档目录下都可以有`config.json`用来配置文档相关页面
* `sidebar.json`: 文档目录

在看如何使用配置文件之前， 一定要牢记， 配置文件很简单， **配置文件共只有两个文件名**， 一个唯一的文档配置文件`site_config` 和 每个文档自己的配置文件`config`

## 配置文件

配置文件可以是 `json` 或者 `yaml` 格式的文件，选择你喜欢使用的即可， 如果没接触过也不要害怕， 搜索一个教程花 10 分钟学习就能完全掌握。

如果你的文档目录内容很多，建议使用`yaml`格式，看起来会更加简洁

`teedoc` 提供了 `json` 和 `yaml` 格式互相转换的命令

### 从 json 转到 yaml

```shell
teedoc -f ./config.json json2yaml
```

### 从 yaml 转到 json


```shell
teedoc -f ./config.yaml yaml2json
```


### 从 gitbook SUMMARY.md 转到 json


```shell
teedoc -f ./SUMMARY.md summary2json
```



### 从 gitbook SUMMARY.md 转到 yaml


```shell
teedoc -f ./SUMMARY.md summary2yaml
```




## site_config.json 网站配置

网站的配置项，比如网站名称，页面路由，插件配置等等

下面的示例配置文件看起来配置项比较多，不要被吓到，其实很简单，主要几个配置项，掌握了就思想就容易了

配置文件是`json`格式， 比如：

```json
{
    "site_name": "teedoc",
    "site_slogon": "happy to write",
    "site_root_url": "/",
    "site_domain": "teedoc.github.io",
    "site_protocol": "https",
    "config_template_dir": "./",
    "source": "https://github.com/teedoc/teedoc.github.io/blob/main",
    "route": {
        "docs": {
            "/get_started/zh/": "docs/get_started/zh",
            "/develop/zh/": "docs/develop/zh",
        },
        "pages": {
            "/": "pages/index/zh",
        },
        "assets": {
            "/static/": "static",
            "/get_started/assets/": "docs/get_started/assets"
        },
        "/blog/": "blog"
    },
    "translate": {
        "docs": {
            "/get_started/zh/": [ {
                    "url": "/get_started/en/",
                    "src": "docs/get_started/en"
                }
            ],
            "/develop/zh/": [ {
                    "url": "/develop/en/",
                    "src": "docs/develop/en"
                }
            ]
        },
        "pages": {
            "/": [ {
                    "url": "/en/",
                    "src": "pages/index/en"
                }
            ]
        }
    },
    "executable": {
        "python": "python3",
        "pip": "pip3"
    },
    "plugins": {
        "teedoc-plugin-markdown-parser":{
            "from": "pypi",
            "config": {
            }
        },
        "teedoc-plugin-theme-default":{
            "from": "pypi",
            "config": {
                "dark": true,
                "env":{
                    "main_color": "#4caf7d"
                },
                "css": "/static/css/custom.css",
                "js": "/static/js/custom.js"
            }
        }
    }
}

```

* `site_name`: 网站名
* `site_slogon`: 网站标语
* `site_root_url`: 网站根目录路径， 使用默认值`/`即可; 如果需要将生成的内容放到网站的文件夹中（不是根目录的文件夹），可以设置
* `site_domain`: 网站域名，目前用到的地方：生成`sitemap.xml` 和 `robots.txt`
* `site_protocol`: 网站协议，`http`或者`https`，目前用到的地方：生成`sitemap.xml` 和 `robots.txt`
* `config_template_dir`: `config` 模板文件，其它文档目录的`config.json`或者`config.yaml`可以`import`这里面的文件，默认位置为`site_config`所在目录
* `source`: 文档源码路径， 比如`https://github.com/teedoc/teedoc.github.io/blob/main`，这里`main`是文档主分支，会在文档页面添加`Edit this page`按钮（链接），点击可跳转到对应文件源码。可以留空，则不添加链接。另外，可以在`config.json`中添加`"show_source": "编辑本页"`来定义按钮的文字内容为`编辑本页`，如果希望文档没有这个按钮，则设置为`"show_source": false`；也可在文件(`md`或`ipynb`文件)的头信息中添加`show_source: 编辑本页`或者 `show_source: false` 来设置
* `route`: 网页路由，包含了文档和页面以及资源文件的路由，比如文档的路由
```json
"docs": {
    "/get_started/zh/": "docs/get_started/zh",
    "/get_started/en/": "docs/get_started/en",
    "/develop/zh/": "docs/develop/zh",
    "/develop/en/": "docs/develop/en"
},
```
`key`代表了最终生成的网站中文档的`url`, 后面的值则是对应的源文档路径，
比如源文档`docs/get_started/zh/README.md`，构建后会生成文件`out/get_started/zh/index.html`, 如果不是`md`文件（即不支持的文件），则会原封不动地拷贝文件，最后`out`目录就是生成的网站

`pages`同理，`assets`则不会进行文档转换，直接拷贝到相应的目录

* `translate`: 翻译， 指定文档对应的翻译版本的`url`和文件路径， 同样，在翻译版本的路径下需要有`config`和`sidebar`配置文件，并且`config`文件中指定`locale`来达到指定被翻译的文档语言， 比如中文可以是`zh`, `zh_CN`, `zh_TW`， 英文是`en`， `en_US` 等。翻译的`sidebar`，以及文档路径需要和源文档一致，如果没有翻译可以不放翻译文件，用户访问没有翻译的页面时会跳转到`no_tanslate.html`提示没有翻译，更多详情请看[国际化 i18n](./i18n.md)
* `executable`: 可执行程序设置， 这里可以设置`python`和`pip`的可执行程序名，在安装插件时会用到
* `plugins`: 插件配置， 主要有名字， 来源， 配置项组成。
名字可以在[github](https://github.com) 搜索`teedoc-plugin`来找到开源的插件，也欢迎你参与编写插件（只需要动 `Python` 语法即可）； 
`from`字段填`pypi`即可，如果插件下载到了本地也可以填写文件夹路径，也可以直接填`git`路径比如`git+https://github.com/*****/******.git`
配置项则由具体的插件决定，比如`teedoc-plugin-theme-default`就有`dark`选项来选择是否启用暗黑主题
* `rebuild_changes_delay`: 检测到文件更改后，延迟多少秒自动重新生成该文档， 浏览器中会自动刷新页面，默认为`3`秒，最短可以设置为`0`秒, 可以使用`teedoc -t 3 serve` 或者 `teedoc --delay serve` 来覆盖这个设置
* `robots`: 自定义`robots.txt`的内容， 比如 `"User-agent": "*"`代表允许所有客户端爬取， 这对 `SEO` 会有影响。 比如不允许爬取`JPEG`图片：`"Disallow": "/.jpeg$"`， 不允许访问`admin`目录:`"Disallow": "/admin"`, 具体和`robots.txt`的格式要求相同
* `layout_root_dir`: 布局模板根目录, 默认为`layout`, 即需要布局模板文件时会自动到这个文件夹下面寻找
* `layout_i18n_dirs`: `layout`的国际化翻译目录，可以是路径比如`locales`，在某些特殊情况下也可以设置多个路径比如`["locales1", "locales2]`。 文件内容可以参考[i18n](./i18n.md#插件国际化)制作

## config.json 文档配置

这是针对每个文档的配置，放在每个文档的根目录， 比如`docs/get_started/zh/config.json`， 各个文档相互独立，可以设置一样的来保持网站导航栏一致

在这里面可以配置每个文档的导航栏， 以及页尾（`footer`）的内容, 也可以设置插件的`config`项，在当前文档会覆盖`site_config.json`中的配置，从而实现不同文档不同语言（国际化/i18n）或者样式等

比如：

```json
{
    "import": "config_zh",
    "id": "teedoc_page",
    "class": "language_zh",
    "locale": "zh-CN",
    "navbar": {
        "title": "teedoc",
        "logo": {
            "alt": "teedoc logo",
            "src": "/static/image/logo.png"
        },
        "home_url": "/",
        "items": [
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
                "url": "https://github.com/neutree/teedoc",
                "label": "github",
                "target": "_blank",
                "position": "right"
            },
            {
                "label": "语言: ",
                "position": "right",
                "type": "selection",
                "items": [
                    {
                        "url": "/get_started/zh/",
                        "label": "中文"
                    },
                    {
                        "url": "/get_started/en/",
                        "label": "English"
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
                        "url": "https://github.com/neutree/teedoc",
                        "target": "_blank"
                    },
                    {
                        "label": "Copyright © 2021 Neucrack",
                        "url": "https://neucrack.com",
                        "target": "_blank"
                    }
                ]
            },
            {
                "label": "源码",
                "items": [
                    {
                        "label": "github",
                        "url": "https://github.com/neutree/teedoc",
                        "target": "_blank"
                    },
                    {
                        "label": "本网站源文件",
                        "url": "https://github.com/teedoc/teedoc.github.io",
                        "target": "_blank"
                    }
                ]
            }
        ],
        "bottom": [
            {
                "label": "*ICP备********号-1",
                "url": "https://beian.miit.gov.cn",
                "target": "_blank"
            },
            {
                "label": "*公网安备**************号",
                "url": "https://beian.miit.gov.cn/#/Integrated/index",
                "target": "_blank"
            }
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
    "show_source": "编辑本页"
}
```
* `import`: 可以从模板文件导入配置，没有后缀的文件名。比如`site_config`中设置了`config_template_dir`为`./`，这里填`"import": "config_zh"`, 则代表从导入`site_config`同目录下的`config_zh.json`（优先）或者`config_zh.yaml`。
然后可以添加当前文档的配置，覆盖模板文件，同样的关键字，修改不同的内容即可，如果是数组(列表)，要替换模板文件的内容，需要在模板文件的数组项中增加`id`关键字，然后修改，如果不指定`id`关键字，则会追加到数组中。比如模板文件`config_zh`：
```json
{
    "locale": "zh-CN",
    "navbar": {
        "title": "teedoc",
        "items": [
            {
                "url": "/get_started/zh/",
                "label": "安装使用",
                "position": "left"
            },
            {
                "id": "language",
                "label": "Language: ",
                "position": "right",
                "items": [
                    {
                        "url": "/zh",
                        "label": "中文"
                    },
                    {
                        "url": "/en",
                        "label": "English"
                    }
                ]
            }
        ]
    }
}
```
具体某个文档的配置文件：
```json
{
    "import": "config_zh",
    "navbar": {
        "title": "teedoc123",
        "items": [
            {
                "id": "language",
                "label": "Language: ",
                "position": "right",
                "items": [
                    {
                        "url": "/get_started/zh",
                        "label": "中文"
                    },
                    {
                        "url": "/get_started/en",
                        "label": "English"
                    }
                ]
            }
        ]
    }
}
```
* `id`: 文档的 `id`， 一般情况下不需要写，会将`id`设置到`config.json` 目录下所有页面的`<html>`标签上。 比如这里设置了`teedoc_page`， 那么这个目录下所有页面都会变成`<html id="teedoc_page"> ... </html>`。 如果`markdown`文件设置了`id`，则会覆盖这个值，即每个页面只能有一个`id`。
* `class`: 文档的 `class`， 一般情况下不需要写，会将`class`设置到`config.json` 目录下所有页面的`<html>`标签上， 多个`class`用空格隔开。 比如这里设置了`language_zh`， 那么这个目录下所有页面都会变成`<html class="language_zh"> ... </html>`。 如果`markdown`文件设置了`class`，则会追加，比如`config.json`中设置了`language_zh`， 在`README.md`中设置了`class: zh_readme`， 则最终是`class="language_zh zh_readme"`。 这个功能方便自定义每个页面的样式，或者不同文档的样式。
* `locale`: 地域编号， 可以从[这里](https://www.science.co.il/language/Locale-codes.php)看到， 比如`zh`, `zh_CN`, `en_US`, `ja` 等。也可以通过程序`babel`获取
```
pip install babel
pybabel --list-locales
```
* `navbar`: 导航栏设置，每个文档都可以单独设置导航栏，要想保持整个网站统一，修改每个配置相同即可。关键字`type`用于第一层，用来表示导航栏的这个标签的类别，取值有：
  * `link`: 普通链接，不写`type`关键字时默认是这个选项
  * `list`: 有子项，会以下拉菜单的形式显示
![](../../assets/images/navbar.png)
  * `selection`: 单选项，比如选择语言。 不写`type`关键字并且有`items`关键字时，默认是这个选项
![](../../assets/images/navbar2.png)
  * `language`: 如果在 `site_config`中设置了`translate`, 则会自动将`language`类型的`items`填充为语言列表，这样就不用我们手动写语言列表了！ 效果和`selection`一样(其实内部代码就是将`language`类型自动替换为`selection`)
![](../../assets/images/navbar2.png)
* `footer`: 网站页脚，分为上下两个部分，上部又可以添加多个栏目，每个栏目里面可以有多个值
* `plugins`: 配置插件的配置项，如果`site_config.json`中已经设置了，会覆盖，即子`config`的优先级更高
* `show_source`: 在`site_config.json`中设置了`source`关键词的前提下，即文档源码路径， 比如`https://github.com/teedoc/teedoc.github.io/blob/main`，这里`main`是文档主分支，会在文档页面添加`Edit this page`按钮（链接），点击可跳转到对应文件源码。设置`"show_source": "编辑本页"`来定义按钮的文字内容为`编辑本页`，不设置默认为`Edit this page`, 如果希望文档没有这个按钮，则设置为`"show_source": false`；也可在文件(`md`或`ipynb`文件)的头信息中添加`show_source: 编辑本页`或者 `show_source: false` 来设置

## sidebar.json 文档目录（侧边栏）设置

这里面设置文档的目录，每个文档一份，相互独立

文件路径使用相对路径，填文件名即可， `README.md` 会被自动转换成`index.html`

另外也可以不写`file`路径，直接`url`， 比如`"url": "/get_started/zh/"`, 同时可以设置`"target":"_blank"` 在新窗口打开，不设置则在当前窗口打开

第一层 `items`中的项目，如果只有`label`，没有`url`，`file`和 `items`， 则会在侧边栏中添加一个分类标记,效果如下：
![](../../assets/images/sidebar.png)

另外, 如果希望目录默认展开, 添加`"collapsed": false`即可

比如：
```yaml
items:
-   label: teedoc 简介
    file: README.md
-   label: 安装 teedoc
    file: install/README.md
-   label: 开始写文档
    file: usage/start.md
-   label: 插件
    file: plugins/README.md
    collapsed: false
    items:
    -   label: 主题插件
        file: plugins/themes.md
    -   label: 其它插件
        file: plugins/others.md
-   label: markdown 语法
    file: syntax/syntax_markdown.md
-   label: 使用了 teedoc 的网站
    file: usage/sites.md
-   label: 更多样例
    items:
    -   label: 二级子目录样例
        items:
        -   label: 三级子目录样例
            items:
            -   label: 文章1
                file: more/example_docs/doc1.md
        -   label: 文章2
            file: more/example_docs/doc2.md
    -   label: 这是一个链接
        url: https://github.com/teedoc/teedoc
        target: _blank
```

或者 `json` 格式

```json
{
    "items":[
        {
            "label": "teedoc 简介",
            "file": "README.md"
        },
        {
            "label": "安装 teedoc",
            "file": "install/README.md"
        },
        {
            "label": "开始写文档",
            "file": "usage/start.md"
        },
        {
            "label": "插件",
            "file": "plugins/README.md",
            "collapsed": false,
            "items":[
                {
                    "label": "主题插件",
                    "file": "plugins/themes.md"
                },
                {
                    "label": "其它插件",
                    "file": "plugins/others.md"
                }
            ]
        },
        {
            "label": "markdown 语法",
            "file": "syntax/syntax_markdown.md"
        },
        {
            "label": "使用了 teedoc 的网站",
            "file": "usage/sites.md"
        },
        {
            "label": "更多样例",
            "items":[
                {
                    "label": "二级子目录样例",
                    "items":[
                        {
                            "label": "三级子目录样例",
                            "items":[
                                {
                                    "label": "文章1",
                                    "file": "more/example_docs/doc1.md"
                                }
                            ]
                        },
                        {
                            "label": "文章2",
                            "file": "more/example_docs/doc2.md"
                        }
                    ]
                },
                {
                    "label": "这是一个链接",
                    "url": "https://github.com/teedoc/teedoc",
                    "target": "_blank"
                }
            ]
        }
    ]
}
```

