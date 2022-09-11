
---
title: 使用 teedoc 时的常见问题
keywords: teedoc, build, error, 出错, 问题, 无法运行
desc: 使用 teedoc 时的常见问题
---


可以在[github issue](https://github.com/teedoc/teedoc/issues?q=) 查看大家提过的问题以及解决方法， 可以提交问题， 也欢迎发现问题修改代码并提交PR


## jinja2.exceptions.TemplateNotFound: base.html.j2

需要更新 `nbconvert`：

```shell
pip3 install --upgrade nbconvert
```

## 插件报错, 代码抛出错误

可以先尝试更新`teedoc`:

```
pip install -U teedoc
```

然后再在相应的文档下面更新插件:

```
teedoc install
```

如果仍然有问题, 可以到 github [提交 issue](https://github.com/teedoc/teedoc/issues/new)


## OSError: [Errno 28] inotify watch limit reached

只会在使用`teedoc serve`时出现， 即文件监测数量达到了系统设置的上限。
一般发生在有软件大量检测了文件，常见的情况有：
* 编辑器如`vscode`打开了有非常多文件的文件夹， 关掉这类软件或者少打开文件（夹）一般就能解决问题， 参考 [error ENOSPC](https://code.visualstudio.com/docs/setup/linux#_visual-studio-code-is-unable-to-watch-for-file-changes-in-this-large-workspace-error-enospc)
* 文档项目内的文档确实太多了，达到了监测的上限，这种情况一般很少出现，如果出现了，需要设置系统的监测上限值，具体方法请自行搜索

## OSError: [WinError 10013] 以一种访问权限不允许的方式做了一个访问套接字的尝试。

一般这种情况重启可以解决。但是我们也可以指定使用别的端口来重新构建本地服务，比如 `teedoc serve --port=4969`
然后再访问[127.0.0.1:4969](127.0.0.1:4969)来查看新构建的界面

