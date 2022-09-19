---
title: teedoc更新历史
keywords: teedoc, markdown, jupyter notebook, html, 文档生成, 替代gitbook, 网站生成, 静态网站, 写文档, 更新历史
desc: teedoc， 将 markdown 或者 jupyter notbook 转换成 html 静态网页， 介绍了 teedoc 的更新历史
---

中文文档可能没来得及更新，最新的历史记录看[history.md](https://github.com/teedoc/teedoc/blob/main/history.md)

所有详细的更新历史可以看[代码提交列表](https://github.com/teedoc/teedoc/commits/main)

## 2022-9-16 v1.31.0

* 支持直接在 html 文件中使用 jinja2 模板，而不需要在 md 文件中使用 layout，更多信息请看文档的 html 语法部分。

## 2022-9-16 v1.30.1

* 优化侧边栏
* Markdown 增加 mermaid 支持
* 忽略所有临时文件
* 更新 markdown 解析器 mistune 到 V2
* 增加 `teedoc-list-files` 工具，用于按文件大小列出文件
* 增加 `teedoc-compare` 工具，用于比较两个目录的文件差异
* 增加 `teedoc-upload` 工具，用于上传文件到远程服务器，支持腾讯云和七牛云
* 优化构建日志
* 新增 Markdown 语法：
  * `tabset` (jupyter 暂不支持)
  * `details`(jupyter 暂不支持)
  * 支持自定义标题 ID 语法 `{#id}`
* 元数据头完全支持 `yaml` 格式，增加 `update` 键用于生成更新历史表格
* 新的 teedoc logo
* 修复长目录无法完全显示的 bug
* 移除 `teedoc install` 命令中安装本地插件的功能
* 优化最后修改日期显示

## 2022-05-08

* 更新主题插件，支持移动端显示页面目录
* 更新博客插件，支持在简介中显示图片，支持 `cover` 元数据键

## 2022-09-19

* 增加了`teedoc-plugin-thumbs-up`插件，用于支持点赞功能，详细请看插件说明页面

## 2022-01-06 v1.26.0

* 增加 `teedoc serve --fast` 模式，这个模式下，只会复制静态资源，不会构建页面，这样你就可以访问页面了，页面会在你访问的时候构建。同时后台会构建所有页面。

## 2022-01-06 v1.25.0

* teedoc-plugin-theme-default 插件支持 `redirect.html` 模板，这个模板用于重定向页面，比如我们想让 `/maixpy` 和 `/maixpy.html` 重定向到 `/soft/maixpy.html`，只需要在 `/soft/maixpy.md` 文件中加入

```markdown
---
layout: redirect
redirect_url: /soft/maixpy/zh/
---
```

* 将 http 服务从 http.server 更换为 flask，以便更好的兼容更多的设备


## 2021-09-18 v1.24.0

* teedoc-plugin-ad-hint 现在支持在特定文档中配置，而不只是全局配置，这样就可以实现消息国际化了，不同语言的文档各自设置自己的就好了
* (开发者)插件现在可以通过 `js_vars` 变量向前端传递变量了

## 2021-09-18 v1.23.1

* 增加 404.html 模板， 并支持 i18n

## 2021-09-8 v1.19.0

* 为插件和模板添加 i18n 支持
* 添加评论插件 teedoc-plugin-comments-gitalk
* 添加打印页面支持
* 为页面标题添加锚点
* 为错误的侧边栏目录项目(sidebar)添加警告日志
* 修复错误：在预览模式下当 url 有`#id`时更改内容时不会自动刷新页面
* 修复搜索索引文件过大的bug
* 修复导航栏列表项 z-index 错误
* 修复 id 为转义字符时的 toc 平滑滚动错误

## 2021-08-7 v1.17.1

* 增加 布局模板(layout template) 自定义支持 (Jinja2)

## 2021-08-3 v1.16.1

* 将 markdown 解析器 从 markdown2 更换为 mistune, 现在构建速度至少是之前的 2 倍以上
* 在文件改动监控中去掉对 .git 文件夹的监控


## 2021-07-22 v1.15.8


* 修复文件在文件管理器重命名后无法出发事件的 bug
* 修复拷贝不存在的文件会崩溃的错误

插件 theme_default: 为 img 标签增加图片查看器


## 2021-05-21 v1.15.0

* 添加 summary2json 和 summary2yaml 命令, 以方便将 gitbook 的 SUMMARY.md 转为 sidebar.yaml 或者 `sidebar.json`
* 目录栏支持拖动变换大小, 通过设置插件 `teedoc-plugin-theme-default` 的 `sidebar_width` 配置来设置目录栏默认宽度 , 比如:
```
"teedoc-plugin-theme-default": {
            "from": "../../plugins/teedoc-plugin-theme-default",
            "config": {
                "env": {
                    "sidebar_width": "300px"
                }
            }
}
```


## 2021-05-21 v1.14.0

优化多线程构建, 构建更迅速一些了. (用多进程代替了多线程)

## 2021-05-21 v1.13.0

在 `sidebar.json` 中, 设置`"collapsed": false`来默认展开显示子目录

## 2021-04-14 v1.12.3

* fix sidebar active error
* optimize navbar list type display
* add navbar list type url support
* add --thread parameter, to set build thread number
* update markdown plugin to v1.0.8, warning when parse markdown error instead of program crash

## 2021-1-28 v1.0.1

基本功能

## 2021-1-16

项目开始



