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

这会根据`site_config.sjon`中的`plugins`的插件设置安装插件

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

## 更多例子

更多请访问: [teedoc.neucrack.com](https://teedoc.neucrack.com/) 或者 [teedoc.github.io](https://teedoc.github.io/)

更多例子访问: [github.com/teedoc/teedoc.github.io](https://github.com/teedoc/teedoc.github.io) 或者 [https://github.com/teedoc/template](https://github.com/teedoc/template) , 或 [sipeed wiki](https://github.com/sipeed/sipeed_wiki)




