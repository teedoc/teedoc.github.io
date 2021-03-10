---
title: teedoc 安装使用
keywords: teedoc, markdown, jupyter notebook, html, 文档生成, 替代gitbook, 网站生成, 静态网站, 安装, 使用
desc: teedoc， 将 markdown 或者 jupyter notbook 转换成 html 静态网页， 介绍了 teedoc 的使用方法
---


## 安装 python3

需要先安装`Python3` （仅支持 `Python3`）

比如在`Ubuntu`上：
```shell
sudo apt install python3 python3-pip git
```

`Windows` 和 `macOS`请到[官网下载](https://www.python.org/downloads/)



## 安装 teedoc

打开终端，输入：

```shell
pip3 install teedoc
```

以后使用以下命令来更新软件：
```shell
pip3 install teedoc --upgrade
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


## 在 github 上基于模板仓库创建新仓库


访问 [https://github.com/teedoc/teedoc.github.io](https://github.com/teedoc/teedoc.github.io)

点击 `Use this template`

![github use template](../../assets/images/github_use_template.jpg)

然后填写新的仓库名称和描述，基于这个仓库新建一个你自己的仓库，现在你获得了和 `teedoc`官网一模一样的仓库了

然后使用`git clone 你的仓库地址`就可以克隆到本地了


