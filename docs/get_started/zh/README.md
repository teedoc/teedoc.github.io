---
title: 开源静态文档网站生成工具 teedoc
keywords: teedoc, markdown, jupyter notebook, html, 文档生成, 替代gitbook, 网站生成, 静态网站, 静态博客, 开源文档生成工具
desc: teedoc，开源静态文档网站生成工具, 将 markdown 或者 jupyter notbook 转换成 html 静态网页， 用于个人或企业建站，建博客，建资料库，wiki等
date: 2022-08-1
---


官网: [teedoc.neucrack.com](https://teedoc.neucrack.com/) 或 [teedoc.github.io](https://teedoc.github.io/)
本文档源文件: [github.com/teedoc/teedoc.github.io](https://github.com/teedoc/teedoc.github.io)
源码: [https://github.com/teedoc/teedoc](https://github.com/teedoc/teedoc) 如果你喜欢这个项目，请务必登录 [github](https://github.com/teedoc/teedoc) 给项目点个 **star**

将 `Markdown` 或者 `Jupyter Notebook` 格式的文档转换为 `HTML` 静态网页

以下场景可使用`teedoc`：
* 建文档网站，并且最好支持放多份文档（比如你有一本叫`Python学习`和`C++学习`两本书，它们都有单独的目录, `teedoc`则是书库）
* 文档和网页页面共存，支持自定义`HTML`页面
* 建`WiKi`网站
* 建个人或者企业知识库
* 建个人或者企业网站
* 博客


如果你在使用过程中遇到了问题, 可以在[这里](https://github.com/teedoc/teedoc/issues) (需要先注册并登录 github) 寻找相似问题, 或者创建 issue

也可以加 [QQ群 566531359](https://jq.qq.com/?_wv=1027&k=2FdCPllm) 讨论


## 特性

- [x] 多文档支持， 将多种文档和网页放到同一个站点
- [x] 使用简单， 跨平台，只依赖 `Python3`
- [x] 不需要数据库，网站全静态页面
- [x] 部署简单， 生成的网站是全静态页面，直接拷贝到服务器或者上传到三方机构即可部署
- [x] 书写简单，使用 [Markdown 语法](./syntax/syntax_markdown.md)编写
- [x] [Jupyter notebook 支持](./syntax/syntax_jupyter.ipynb)
- [x] HTML 支持，可直接使用 HTML 编写页面， 并且支持 Jinja2 模板语法，自由度大
- [x] 插件支持，默认提供了一些插件比如顶栏通知、搜索、评论、统计、google翻译页面等
- [x] 多主题支持（由插件实现）
- [x] 通过 css 控制精确到页的样式（通过自定义每页的 id 和 class 实现）
- [x] 多级目录支持（无穷级）
- [x] 多语言支持（手动翻译）(国际化/i18n)
- [x] 多语言支持（翻译插件）
- [x] 多版本支持（实现方法同多语言）
- [x] 搜索支持
- [x] SEO 友好
- [x] 实时预览更改
- [x] 并行构建，更快的构建速度
- [x] 博客支持
- [x] gitbook 切换过来友好, 只需要设置好 `route` 和使用命令(`summary2yaml`)转换目录文件(`SUMMARY.md`)即可
- [x] 评论(插件), 比如 `gitalk`

## 演示

[本网站](https://teedoc.github.io/)即采用 `teedoc` 生成，你现在看到的就是生成后的网站的样子。

另外还有其它网站使用了`teedoc`，具体请看[这里](./usage/sites.md)


## 类似的工具

实际上这种类型的工具已经有很多了，但是每一个又稍微不同，按照自己的需求选择一个就好了

如果你有选择困难症，那么符合以下部分条件，都建议使用 teedoc：
* 用来做文档网站，或者想把企业或组织官网+手里的多份文档统一放到一个网站？还各种语言的网站？ 用 `teedoc` 吧
* 使用 `Jupyter notebook` 写文档或代码？ 果断选择 `teedoc`
* 功能符合你的需求吗？
* 界面符合你的审美吗（可以自定义 `css`， 或者换主题插件）
* 对 `Python` 熟悉？ 可以随时自定义插件和功能

其它类似工具：
* `docusaurus`: `teedoc` 的 `UI` 布局几乎和它类似，不过它使用 `ract` 写的， `teedoc` 是原生 `js`, 如果你精通 `ract` 可以考虑用这个，新手可能用起来比较吃力
* `gitbook`: 曾经很好用的工具，但是官方不维护了，现在只有商业化网站了，转向商业了，不建议再使用，特别是国内可能无法访问
* `docsify`: 只需要一个页面，`markdown` 在浏览器渲染，而不是预先渲染成 `HTML`， 好处就是轻量，但是 `SEO` 不太友好，可以用它的 `SSR` 功能， `nodejs` 编写
* `readthedocs(Sphinx)`: 其实是用了用 `Sphinx` 做为生成工具，`Python` 官网文档就是这个工具生成的， 很多开源项目使用的工具，`readthedocs` 只是一个公开文档的网站，你不用自己搭建网站，注册登录就可以开始写文档，对 `RST` 格式支持友好，如果你的文档比较单一可以考虑使用
* `mkdoc`: 也是一个 `python` 写的工具，简单易上手，插件也多，如果你的文档是单一语言的文档，可以使用这个工具


## 一些使用建议

* 在 footer 添加 [使用 teedoc 生成](https://github.com/teedoc/teedoc)， 帮助更多人发现 teedoc，促进项目的成长
* 使用模板项目开始一个新的文档项目，可以先跑起来，然后再根据自己的需求修改，这样上手更快哦

## 快速开始

查看[快速开始](./usage/quick_start.md)来安装并使用吧！



