---
title: Quickly convert from gitbook to teedoc
keywords: gitbook, teedoc
desc: Quickly convert from gitbook to teedoc, get rid of gitbook single-threaded construction turtle speed
---


Introduce how to quickly convert from gitbook to teedoc, because gitbook has stopped maintenance, and some features are no longer suitable for current use, such as single-threaded construction, which is very slow.

Take [re0-web](https://github.com/lyy289065406/re0-web/tree/033f5dfd7c9d3b8654b3c1ea4dabfab8b3ebebb7) as an example:


The author said in the readme that it takes 30 minutes to build once, but after looking at the source code, the number of documents is not a lot, and it shouldn't take so long.After seeing that the author uses docker, and it is under windows, it is naturally very slow. The solution is to switch to the linux build, which will speed up some speed, and it is estimated that the build can be completed in 5 minutes.

Teedoc has the advantage of multi-threaded construction, and other functions and UI can also be satisfied, you can consider switching to teedoc (but teedoc does not support epub and pdf export (2021-05-29), if it is supported, it will be in [here](../README.md) description)


Final effect: [teedoc.github.io/re0-web-teedoc/](https://teedoc.github.io/re0-web-teedoc/)

## New teedoc project

```
mkdir re0-web-teedoc
cd re0-web-teedoc
teedoc init
teedoc install
```

Use the `teedoc serve` command to start the build and start an http service, visit `http://127.0.0.1:2333` to preview

Use the `teedoc build` command to build a version that can be deployed to the server, the file is in the `out` directory, you can delete the `out` directory before building


## Copy source file


The source files of the `re0-web` project are under the `gitbook/markdown` directory, the resource files (pictures) are all placed in `gitbook/res`, and the directory files on the left are in `gitbook/SUMMARY.md`

Other files are not important, they are all project files or generated files, so we only need to copy them

For example, create a folder `books/re0`, then copy the contents of the original project `gitbook/markdown/ch/` directory to the `books/re0/ch/` directory, and copy the original project `gitbook/markdown/jp/` Copy the contents of the directory to the `books/re0/jp/` directory, and copy the original projects `gitbook/markdown/character.md` and `gitbook/README.md` to `books/re0/ch/` and `gitbook/markdown/jp/`

In addition, create a folder `books/res/` for the resource files, copy all the files in the original project `gitbook/res/` directory to the `books/res/` directory


## Create configuration for each document

Copy the `config.json` and `sidebar.yaml` files from the `docs/get_started/zh/` directory to `books/re0/ch/` and `books/re0/jp/`

Each directory with `config.json` is equivalent to **a document**, or **a book**



## Modify site_config.json

* Modify route

Edit `site_config.json`, find the key value of `route`, modify the content:
```json
     "route": {
        "docs": {
            "/gitbook/book/markdown/ch/": "books/re0/ch",
            "/gitbook/book/markdown/jp/": "books/re0/jp"
        },
        "pages": {
            "/": "pages/index/zh",
            "/en/": "pages/index/en"
        },
        "assets": {
            "/static/": "static",
            "/res/": "books/res"
        },
```

Here `url` is compatible with the previous path `gitbook/book/markdown/` and the resource path `/res/`.

Because `re0-web` project source file pictures use absolute path `url` such as `<img width="500" src="/res/img/article/chapter010/12.jpg" />`, So here `/res/` is alluded to the `books/res` folder;

In addition, a relative path can be used in the source file, but the directory must be in the same document, such as in this document (click on the upper right corner to edit this page to see the source code), `![](../assets/images/logo. png)`: ![](../assets/images/logo.png)

Because the `assets` directory is also under the `get_started/zh/` directory, it can be directly referenced, but directories beyond the scope of this document cannot be directly referenced by relative paths.


But there are also ways to achieve it, such as quoting the resources in the `get_started/assets/` directory here, which can also be referenced in the `get_started/zh/` directory, just need to configure the `route` cleverly, as follows:
default
```json
    "route": {
        "docs": {
            "/get_started/zh/": "docs/get_started/zh",
        },
        "assets": {
            "/get_started/assets/": "docs/get_started/assets"
        },
```

* Modify other

Modify `title` `domain` `source` etc.


## Sidebar Directory

* View documentation

If you use `teedoc serve` at this time, you can actually see the document at `http://127.0.0.1:2333/gitbook/book/markdown/ch/character.html`

But the list on the left is not finished yet, now modify the left list

* Convert SUMMARY.md

Enter `books/re0/ch/`, earlier we copied a file of `sidebar.yaml`, now we need to convert the `SUMMARY.md` of gitbook into this file format

Find the `SUMMARY.md` file of the previous `gitbook` project, and execute

```shell
teedoc -f'SUMMARY.md' summary2yaml
```

We get a `sidebar.yaml` file, copy it to the `books/re0/ch/` directory and overwrite the previous `sidebar.yaml`

* Modify the file path of sidebar.yaml

Because the previous directory structure may not be similar to the directory structure of the new project, you need to modify the file path in `sidebar.yaml`, for example:

`markdown/character.md` is changed to `character.md`, which is the path relative to the document (the directory with `config.json`)
`markdown/ch/chapter010/README.md` is changed to `chapter010/README.md`, you can batch replace `markdown/ch/` as empty, which can be done in many editors, such as `vscode` and press `Ctrl +H` key to modify

If you want the link (`url`) to open in a new window, just add a `target: _blank` key value.


* View the effect

Then execute `teedoc serve` to visit `http://127.0.0.1:2333/gitbook/book/markdown/ch/index.html` and you can see the directory on the left, and you can get the same for the directory of `jp`


## Edit navigation bar

The navigation bar needs to be edited as needed

### Edit template `config/config_zh.json`

```json
{
    "class": "language_zh",
    "navbar": {
        "title": "re0-web",
        "logo": {
            "alt": "re0-web logo",
            "src": ""
        },
        "home_url": "/",
        "items": [
            {
                "url": "/gitbook/book/markdown/ch/",
                "label": "Chinese",
                "position": "left"
            },
            {
                "url": "/gitbook/book/markdown/jp/",
                "label": "日本语",
                "position": "left"
            },
            {
                "url": "https://github.com/teedoc/re0-web-teedoc",
                "label": "github",
                "target": "_blank",
                "position": "right"
            },
            {
                "id": "language",
                "label": "Language: ",
                "position": "right",
                "items": [
                    {
                        "id": "zh",
                        "url": "/",
                        "label": "Chinese"
                    },
                    {
                        "id": "en",
                        "url": "/jp/",
                        "label": "日本语"
                    }
                ]
            }
        ]
    },
    "footer":{
        "top":[
            {
                "label": "Link",
                "items": [
                    {
                        "label": "Use teedoc to build",
                        "url": "https://teedoc.neucrack.com",
                        "target": "_blank"
                    },
                    {
                        "label": "Site Map",
                        "url": "/sitemap.xml"
                    }
                ]
            },
            {
                "label": "Source",
                "items": [
                    {
                        "label": "Source files of this website",
                        "url": "https://github.com/teedoc/re-web-teedoc",
                        "target": "_blank"
                    }
                ]
            }
        ],
        "bottom": [
        ]
    },
    "plugins": {
        "teedoc-plugin-search":{
            "config": {
                "search_hint": "Search",
                "input_hint": "Enter keywords, separate multiple keywords with spaces",
                "loading_hint": "Loading, please wait...",
                "download_err_hint": "Failed to download file, please refresh and try again or check the network",
                "other_docs_result_hint": "Results from other documents",
                "curr_doc_result_hint": "Current document search result"
            }
        }
    },
    "show_source": "Edit this article"
}
```

Another `config_en.json` can be renamed to `config_jp.json`, similarly modify the following content, by the way, rename `pages/index/en` to `pages/index/jp`

```json
{
    "navbar": {
        "title": "re0-web",
        "logo": {
            "alt": "re0-web logo",
            "src": ""
        },
        "home_url": "/jp/",
        "items": [
            {
                "url": "/gitbook/book/markdown/ch/",
                "label": "Chinese",
                "position": "left"
            },
            {
                "url": "/gitbook/book/markdown/jp/",
                "label": "日本语",
                "position": "left"
            },
            {
                "url": "https://github.com/neutree/re0-web-teedoc",
                "label": "github",
                "target": "_blank",
                "position": "right"
            },
            {
                "id": "language",
                "label": "Language: ",
                "position": "right",
                "items": [
                    {
                        "id": "zh",
                        "url": "/",
                        "label": "Chinese"
                    },
                    {
                        "id": "en",
                        "url": "/jp/",
                        "label": "日本语"
                    }
                ]
            }
        ]
    },
    "footer":{
        "top":[
            {
                "label": "Links",
                "items": [
                    {
                        "label": "Site generated by teedoc",
                        "url": "https://teedoc.neucrack.com",
                        "target": "_blank"
                    },
                    {
                        "label": "Sitemap",
                        "url": "/sitemap.xml"
                    }
                ]
            },
            {
                "label": "Source code",
                "items": [
                    {
                        "label": "website source files",
                        "url": "https://github.com/teedoc/re0-web-teedoc",
                        "target": "_blank"
                    }
                ]
            }
        ],
        "bottom": [
        ]
    }
}
```

### Modify the configuration file config.json of each book (/document)

* Modify `books/re0/ch/config.json`

```json
{
    "import": "config_zh",
    "navbar": {
        "items": [
            {
                "id": "language",
                "label": "Language: ",
                "position": "right",
                "items": [
                    {
                        "id": "zh",
                        "url": "/gitbook/book/markdown/ch/",
                        "label": "Chinese"
                    },
                    {
                        "id": "en",
                        "url": "/gitbook/book/markdown/jp/",
                        "label": "日本语"
                    }
                ]
            }
        ]
    }
}
```

* Modify `books/re0/jp/config.json`

```json
{
    "import": "config_jp",
    "navbar": {
        "items": [
            {
                "id": "language",
                "label": "Language: ",
                "position": "right",
                "items": [
                    {
                        "id": "zh",
                        "url": "/gitbook/book/markdown/ch/",
                        "label": "Chinese"
                    },
                    {
                        "id": "en",
                        "url": "/gitbook/book/markdown/jp/",
                        "label": "日本语"
                    }
                ]
            }
        ]
    }
}
```

## Edit home page

Edit `pages/index/zh/README.md`, modify as needed


## Deployment

Like `gitbook`, `teedoc build` generates static files and puts them in the `out` folder, and can be directly copied to the server for deployment. For more information, please see [document](./deploy.md)

Final effect: [teedoc.github.io/re0-web-teedoc/](https://teedoc.github.io/re0-web-teedoc/)

