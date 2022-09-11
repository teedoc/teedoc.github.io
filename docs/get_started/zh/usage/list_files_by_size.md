---
title: 按照文件大小顺序列出所有文件/找出大文件
keywords: 找出目录下大文件, 文件大小排序
desc: 使用 teedoc 工具 teedoc-list-files 命令来按照目录下文件大小列出所有文件，方便查找出大文件
---

使用 `teedoc` 中的工具 `teedoc-list-files` 命令来按照目录下文件大小列出所有文件，方便查找出大文件

使用帮助：
```
teedoc-list-files -h
```

比如需要将目录`my_doc`下的所有文件列出，并按照文件大小排序，可以这样使用：

```
teedoc-list-files -r my_doc all
```

这里 `-r` 是递归查找，即会查找子文件夹， `all` 代表所有文件格式，也可以指定其它格式，比如：

```
teedoc-list-files -r my_doc jpg
```

就会看到排序过后的文件列表了
```
402.1 KiB - 411708 : get_started/assets/images/jupyterlab.jpg
234.2 KiB - 239864 : get_started/assets/images/pages_settings.jpg
215.7 KiB - 220828 : get_started/assets/images/action_error.jpg
165.8 KiB - 169815 : get_started/assets/images/action_error_log.jpg
139.6 KiB - 142970 : get_started/assets/images/jupyterlab_1.jpg
```


如果你想要彻底删除文件及其历史记录，参考 [github 文档](https://docs.github.com/en/authentication/keeping-your-account-and-data-secure/removing-sensitive-data-from-a-repository) 或者 [gitee 文档](https://gitee.com/help/articles/4232#article-header2)



