---
title: write document content
keywords: teedoc, teeodc write document content, teedoc markdown
desc: teedoc, which converts markdown or jupyter notbook into html static web pages, introduces teedoc’s document content writing specifications
---


The file placed under the document directory, if it is a recognizable file, such as `*.md`, it will be converted to `*.html`, if it cannot be recognized, such as `*.jpg`, it will be copied intact To the output folder (`out`)

`Markdown` parsing is done by the plug-in `teedoc-plugin-markdown-parser`, which is used by default in the template project.
The `jupyter-notebook` plugin `teedoc-plugin-jupyter-notebook-parser` may not be used by default, it needs to be configured in the `site_config` file


## Use `Markdown` to write documents

* Create a file in `.md` format under the document directory (the directory containing the `config.json` or `config.yaml` configuration file) (you can create subdirectories at will), and use the `UTF-8` file format ( Try not to use the default Notepad of `Windows`, it is recommended to use [vscode](https://code.visualstudio.com/) + a `markdown` plugin, or other `Markdown` editors such as [typora](https: //www.typora.io/), or a plain text editor [notepad++](https://notepad-plus-plus.org/))
* To add header content (`metadata`) to the file, at least the key value of `title` is required, for example:

```markdown
---
title: markdown syntax
tags: teedoc, markdown, syntax
keywords: teedoc, markdown, syntax
desc: teedoc's markdown syntax introduction and examples
---
```

* Check [Markdown Syntax](../syntax/syntax_markdown.md) to learn more about the syntax and details of using `Markdown` to write documents. If you have used `Markdown`, it is strongly recommended to spend 2 minutes browsing it!
* If you need to link the document to the left directory, configure it in `sidebar.json` or `sidebar.yaml`


## Use `Jupyter notebook` file

The plug-in `teedoc-plugin-jupyter-notebook-parser` is required, which may not be used by default and needs to be configured in the `site_config` file.

Then copy the files in `.ipynb` format to the document directory, see [Jupyter syntax](../syntax/syntax_jupyter.ipynb) for specific examples



## Links, pictures and other resource files

Resource files will be copied to the output folder (`out`), so the most important thing is how to reference in the document

### The easiest and recommended way

Use relative paths:

Resource files can be placed in the corresponding directory of the document, such as the document `docs/get_started/zh`, you can create `docs/get_started/zh/assets/images/logo.png`,
Then use relative path references in `docs/get_started/zh/README.md`, namely `![](assets/images/logo.png)`

### Advanced method

This situation applies to multiple documents referencing resources in the same folder (`url`), which is convenient for maintaining multiple documents, such as multilingual translation, or reducing traffic consumption of `CDN`

Use resources outside the document path, configure in `site_config.json`
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
This setting will copy the entire directory of `docs/get_started/assets` to `/get_started/assets`
So only need to use relative path reference in `docs/get_started/zh/README.md`, namely `![](../assets/images/logo.png)`

