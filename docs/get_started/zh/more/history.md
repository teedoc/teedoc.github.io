---
title: teedoc更新历史
keywords: teedoc, markdown, jupyter notebook, html, 文档生成, 替代gitbook, 网站生成, 静态网站, 写文档, 更新历史
desc: teedoc， 将 markdown 或者 jupyter notbook 转换成 html 静态网页， 介绍了 teedoc 的更新历史
---


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



