---
title: teedoc 快速开始
keywords: teedoc 使用, teedoc 快速开始
desc: teedoc， 将 markdown 或者 jupyter notbook 转换成 html 静态网页， 介绍了 teedoc 的快速上手使用方法
---

本篇文档主要是为了让刚接触的你快速上手， 更多请看后面更详细的文档

## 安装 Python3

`teedoc` 是基于`Python3`语言开发的软件，需要有这个软件的支持

比如在`Ubuntu`上：

```shell
sudo apt install python3 python3-pip git
```

`Windows` 和 `macOS`请到[官网下载](https://www.python.org/downloads/)


## 安装 teedoc

打开终端(`Windows`按`Ctrl+R`输入`cmd`)，输入：

```shell
pip3 install teedoc
```

以后使用以下命令来更新软件

>! 使用前务必同时更新软件和插件再使用，以防版本不同导致出现问题

```shell
pip3 install -U teedoc
```

> 如果你的网络使用 `pypi.org` 速度很慢，可以选择其它源，比如清华 tuna 源： `pip3 install teedoc -i https://pypi.tuna.tsinghua.edu.cn/simple`

现在你可以在终端使用 `teedoc` 命令了

如果不能，请检查是不是`Python`可执行目录没有加入到环境变量 `PATH`,
比如可能在 `~/.local/bin`

## 新建工程

新建一个空目录用来放文档工程

```shell
mkdir my_site
cd my_site
teedoc init
```

或者
```shell
teedoc -d my_site init
```

选择`1`，也就是`minimal`模板进行生成， 也可以直接`teedoc -d my_site --template=minimal init`进行生成

这会在 `my_site` 目录下自动生成一些基础文件


另外，除了使用`init`命令生成一份最小工程，你也可以获得一份官网文档源码，基于这个文档内容修改
```shell
git clone https://github.com/teedoc/teedoc.github.io my_site
```
或者
```shell
git clone https://gitee.com/teedoc/teedoc.gitee.io my_site
```

## 安装插件

这会根据`site_config.json`中的`plugins`的插件设置安装插件

```shell
cd my_site
teedoc install
```

> 插件也是以 `python` 包的形式发布的， 所以这会从 `pypi.org` 下载对应的插件，同样，也可以使用其它源，比如清华 tuna 源： `teedoc -i https://pypi.tuna.tsinghua.edu.cn/simple install`

<span></span>

>! 使用前务必同时更新软件和插件再使用，以防版本不同导致出现问题

## 构建 `HTML` 页面并起一个`HTTP`服务

```shell
teedoc serve
```

这个命令会先构建所有`HTML`页面以及拷贝资源文件，然后起一个`HTTP`服务
如果只需要生成页面，使用

```shell
teedoc build
```


在显示 `Starting server at 0.0.0.0:2333 ....` 后，就可以了

打开浏览器访问: [http://127.0.0.1:2333](http://127.0.0.1:2333)


同时可以看到目录下多了一个`out`目录，里面就是生成的静态网站内容，直接拷贝到服务器使用`nginx`或者`apache`进行部署即可


## 文档结构

因为 `teedoc` 特别为 多文档系统 设计， **有个基本概念， 每个文档工程包含了多份文档， 每份文档都有自己的配置文件名为 config** 需要先牢记

工程里面有几个重要文件：
* 工程根目录有`site_config.json`文件， 是工程的主要配置
* 工程里面可以有多份文档，在`site_config`的`route`配置项中设置，每份文档目录下面必须有`config.json`和`sidebar.json`(`json`文件也可以是`yaml`文件)， `config`文件负责这份文档的配置项，比如文档名称，多份文档可以使用`import`公用一份模板

## 添加一篇文档

* 在本文件所在目录创建 `markdown`（以  `.md` 结尾的）文件，比如 `first.md`，添加内容

每篇文章开头都可以有一个`元数据区`， 用以配置文章的相关配置， 至少需要一个`title`即标题，更多配置项和文档格式请[阅读文档](../syntax/syntax_markdown.md)

```markdown
---
title: title
---

## 标题一

内容一

## 标题二

内容二
```

如果没有`元数据区`，则至少需要一个一级标题作为文章标题，比如：
```markdown
# 文章标题

## 标题一

内容一
```
或者
```markdown
文章标题
===

## 标题一

内容一
```


* 在 `sidebar.yaml` 中添加侧边栏链接

```markdown
items:
-   label: Brief
    file: README.md
-   label: First
    file: first.md
```

## 使用图片

在`.md`文件中使用图片，有三种方法：

* 直接引用 `url`， 比如 `https://teedoc.github.io/static/image/logo.png` 或者 `/static/image/logo.png`

* 相对路径引用图片文件。 比如 `./assets/logo.png`. 比如
```
doc1
├── assets
     └── logo.png
├── config.json
├── README.md
└── sidebar.yaml
```
这是工程中的一份文档，下面有`config`配置文件和`sidebar`文件.
直接在`README.md`文件中引用`![logo](./assets/logo.png)` 即可。
**需要注意的是， 只能引用当前文档内文件夹的图片**，不能用相对路径引用这份文档以外的图片

* 如果需要引用当前这份文档之外的路径的资源，可以通过设置路径映射（`route`）实现，比如在`docs`目录下有文件：
```
docs
└── assets
     └── logo.png
      doc1
      ├── config.json
      ├── README.md
      └── sidebar.yaml
static
```
我们在`README.md`文件中引用`![logo](../assets/logo.png)` ，会发现图片没法显示

要让这种引用能够使用， 需要在`site_config`中设置
```json
"route": {
    "docs": {
        "/doc1/": "docs/doc1"
    },
    "assets": {
        "/static/": "static",
        "/assets/": "docs/assets"
    }
}
```
这样设置就可以使用了。 
> 原因是： 我们设置了`docs/doc1`下的文档渲染后拷贝到`out/doc1`目录，`docs/assets`拷贝到`out/assets`，所以在`out/doc1`下面的文档直接使用相对路径就可以引用`out/assets`目录的资源文件了

## 设置地区

设置文档地区，以让某些页面和文字显示为对应的语言， 比如搜索插件会根据文档地区生成对应的搜索提示等

在`config/config.json`文件中，修改`"locale": "en"`为实际使用的地区， 比如`zh`, `zh_CN`, `zh_TW`, `en_US`, `ja`等， 更多看[i18n文档](i18n.md)

## 更多例子

更多请访问: [teedoc.neucrack.com](https://teedoc.neucrack.com/) 或者 [teedoc.github.io](https://teedoc.github.io/)

更多例子访问: [github.com/teedoc/teedoc.github.io](https://github.com/teedoc/teedoc.github.io) 或者 [https://github.com/teedoc/template](https://github.com/teedoc/template) , 或 [sipeed wiki](https://github.com/sipeed/sipeed_wiki)




