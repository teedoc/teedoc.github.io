---
title: teedoc quick start
keywords: teedoc use, teedoc quick start
desc: teedoc, which converts markdown or jupyter notbook into html static web pages, introduces the quick and easy way to use teedoc
---

This document is mainly to let you who are new to contact you quickly get started. For more details, please see the more detailed documents later.

## Install Python3

`teedoc` is a software developed based on `Python3` language, and it needs the support of this software

For example, on `Ubuntu`:

```shell
sudo apt install python3 python3-pip git
```

`Windows` and `macOS` please go to [Official Website Download](https://www.python.org/downloads/)


## Install teedoc

Open the terminal (`Windows` press `Ctrl+R` and enter `cmd`), enter:

```shell
pip3 install teedoc
```

Use the following command to update the software later

>! Be sure to update the software and plug-ins at the same time before use, to prevent problems caused by different versions

```shell
pip3 install -U teedoc
```

> If your network uses `pypi.org` and the speed is very slow, you can choose other sources, such as Tsinghua tuna source: `pip3 install teedoc -i https://pypi.tuna.tsinghua.edu.cn/simple`

Now you can use the `teedoc` command in the terminal

If not, please check if the `Python` executable directory is not added to the environment variable `PATH`,
For example, it may be in `~/.local/bin`

## New Project

Create an empty directory to store the document project

```shell
mkdir my_site
cd my_site
teedoc init
```

or
```shell
teedoc -d my_site init
```

Select `1`, which is the `minimal` template to generate, or you can directly generate it with `teedoc -d my_site --template=minimal init`

This will automatically generate some basic files in the `my_site` directory


In addition, in addition to using the `init` command to generate a minimal project, you can also get a source code of the official website document and modify it based on the content of this document
```shell
git clone https://github.com/teedoc/teedoc.github.io my_site
```
or
```shell
git clone https://gitee.com/teedoc/teedoc.gitee.io my_site
```

## Install plugin

This will install the plugin according to the plugin settings of `plugins` in `site_config.json`

```shell
cd my_site
teedoc install
```

> The plug-in is also released in the form of `python` package, so this will download the corresponding plug-in from `pypi.org`. Similarly, other sources can also be used, such as Tsinghua tuna source: `teedoc -i https://pypi. tuna.tsinghua.edu.cn/simple install`

>! Be sure to update the software and plug-ins at the same time before use, to prevent problems caused by different versions

## Build an `HTML` page and start an `HTTP` service

```shell
teedoc serve
```

This command will first build all `HTML` pages and copy resource files, and then start an `HTTP` service
If you only need to generate pages, use

```shell
teedoc build
```


After displaying `Starting server at 0.0.0.0:2333 ....`, it is fine

Open the browser to visit: [http://127.0.0.1:2333](http://127.0.0.1:2333)


At the same time, you can see that there is an additional `out` directory under the directory, which is the generated static website content, directly copy it to the server and use `nginx` or `apache` for deployment.


## Document structure

Because `teedoc` is specially designed for multi-document system, **There is a basic concept, each document project contains multiple documents, and each document has its own configuration file named config** You need to keep in mind first

There are several important files in the project:
* The project root directory has a `site_config.json` file, which is the main configuration of the project
* There can be multiple documents in the project, set in the `route` configuration item of `site_config`, each document directory must have `config.json` and `sidebar.json` (`json` file can also be `yaml `File), `config` file is responsible for the configuration items of this document, such as the document name, multiple documents can use `import` to share a template

## Add a document

* Create a `markdown` (end with `.md`) file in the directory where this file is located, such as `first.md`, and add content

At the beginning of each article, there can be a `metadata area` to configure the related configuration of the article. At least one `title` is required. For more configuration items and document formats, please [read the document](../syntax/syntax_markdown.md)

```markdown
---
title: title
---

## Title

content

## Title Two

Content two
```

If there is no `metadata area`, at least one first-level title is required as the article title, such as:
```markdown
# Article title

## Title One

Content One
```
or
```markdown
Article title
===

## Title One

Content One
```

* Add sidebar link in `sidebar.yaml`

```markdown
items:
-label: Brief
    file: README.md
-label: First
    file: first.md
```

## Use pictures

There are three ways to use pictures in `.md` files:

* Directly quote `url`, such as `https://teedoc.github.io/static/image/logo.png` or `/static/image/logo.png`

* The relative path refers to the picture file. For example, `./assets/logo.png`. For example
```
doc1
├── assets
     └── logo.png
├── config.json
├── README.md
└── sidebar.yaml
```
This is a document in the project, there are `config` configuration files and `sidebar` files below.
Directly quote `![logo](./assets/logo.png)` in the `README.md` file.
**It should be noted that you can only refer to the pictures of the folder in the current document**, and you cannot use relative paths to refer to pictures outside of this document

* If you need to refer to the resources of the path outside the current document, you can set the path mapping (`route`) to achieve, for example, there are files in the `docs` directory:
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
When we quote `![logo](../assets/logo.png)` in the `README.md` file, we will find that the image cannot be displayed

To make this kind of reference available, you need to set it in `site_config`
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
This setting can be used.
> The reason is: We set the document under `docs/doc1` to render and copy it to the directory `out/doc1`, and copy `docs/assets` to `out/assets`, so the file under `out/doc1` The document directly uses the relative path to reference the resource files in the `out/assets` directory

## Locale setting

Set the document Locale so that certain pages and texts are displayed in the corresponding language, for example, the search plugin will generate corresponding search prompts based on the document Locale, etc.

In the `config/config.json` file, modify `"locale": "en"` to the actual Locale used, such as `zh`, `zh_CN`, `zh_TW`, `en_US`, `ja`, etc., more See more [i18n document](i18n.md)

## More examples

For more information, please visit: [teedoc.neucrack.com](https://teedoc.neucrack.com/) or [teedoc.github.io](https://teedoc.github.io/)

For more examples, visit: [github.com/teedoc/teedoc.github.io](https://github.com/teedoc/teedoc.github.io) or [https://github.com/teedoc/template]( https://github.com/teedoc/template), or [sipeed wiki](https://github.com/sipeed/sipeed_wiki)

