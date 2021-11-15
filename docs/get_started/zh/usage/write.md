---
title: 编写文档内容
keywords: teedoc, teeodc 编写文档内容, teedoc markdown
desc: teedoc， 将 markdown 或者 jupyter notbook 转换成 html 静态网页， 介绍了 teedoc 的文档内容编写规范
---


放在文档目录下面的文件，如果是可是识别的文件，比如`*.md`， 则会转换成`*.html`， 如果不能识别，比如`*.jpg`， 则会原封不动地拷贝到输出文件夹（`out`）

`Markdown` 解析由插件`teedoc-plugin-markdown-parser`完成， 模板工程默认使用了该插件。
`jupyter-notebook`插件`teedoc-plugin-jupyter-notebook-parser`默认可能没有使用， 需要在`site_config`文件中配置


## 使用 `Markdown` 编写文档

* 在文档目录（包含了`config.json`或者`config.yaml`配置文件的目录）下创建一个`.md`格式的文件（可以随便建立子目录），使用`UTF-8`文件格式（尽量不要使用`Windows`默认的记事本， 推荐使用[vscode](https://code.visualstudio.com/) + 一款`markdown`插件, 或者其它`Markdown`编辑器比如[typora](https://www.typora.io/)， 或者纯文本编辑器[notepad++](https://notepad-plus-plus.org/)）
* 给文件添加头内容（`metadata`）, 至少需要`title`键值， 比如：

```markdown
---
title: markdown 语法
tags: teedoc, markdown, 语法
keywords: teedoc, markdown, 语法
desc: teedoc 的 markdown 语法介绍和实例
---
```

* 查看[Markdown 语法](../syntax/syntax_markdown.md)了解更多使用`Markdown`编写文档的语法和细节， 如果你使用过`Markdown`， 也强烈建议花 2 分钟浏览一遍！
* 如果你需要将文档链接到左侧目录， 则在`sidebar.json`或者`sidebar.yaml`中配置


## 使用`Jupyter notebook` 文件

需要插件`teedoc-plugin-jupyter-notebook-parser`， 默认可能没有使用， 需要在`site_config`文件中配置。

然后复制`.ipynb`格式的文件到文档目录即可， 具体的样例见[Jupyter 语法](../syntax/syntax_jupyter.ipynb)



## 链接、图片等资源文件

资源文件会被拷贝到输出文件夹（`out`）, 所以最重要的是怎么在文档中引用

### 最简单和推荐的方法

使用相对路径：

资源文件可以放在文档对应的目录，比如文档`docs/get_started/zh`， 可以创建`docs/get_started/zh/assets/images/logo.png`,
然后在`docs/get_started/zh/README.md`中使用相对路径引用，即`![](assets/images/logo.png)`

### 进阶方法

这种情况适用于多份文档都引用同一个文件夹下（`url`）的资源， 方便维护多份文档，比如多语言翻译，或者减少 `CDN` 流量消耗

使用文档路径外的资源，在`site_config.json` 中配置
```json
{
    "route": {
        "docs": {
            "/get_started/zh/": "docs/get_started/zh",
        },
        "assets": {
            "/get_started/assets/": "docs/get_started/assets"
        }
    }
}
```
这个设置会将`docs/get_started/assets`整个目录拷贝为`/get_started/assets`
所以只需要在`docs/get_started/zh/README.md`中使用相对路径引用，即`![](../assets/images/logo.png)`


