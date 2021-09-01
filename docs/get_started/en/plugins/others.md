---
title: teedoc other plugins
keywords: teedoc, theme plugin, themes, plugin
desc: teedoc other plugins
---

## `teedoc-plugin-markdown-parser`: default markdown parsing plugin

Configure the plugin in `site_config.json`
```json
    "plugins": {
        "teedoc-plugin-markdown-parser":{
            "from": "pypi",
            "config": {
                "toc_depth": 3
            }
        },
    }
```

* `toc_depth`: The depth of the article table of contents (right column), the default is `3`, which means to `h3` that is `### three-level heading` in `markdown`

## `teedoc-plugin-jupyter-notebook-parser`: jupyter notebook parsing plugin

Configure the plugin in `site_config.json`
```json
    "plugins": {
        "teedoc-plugin-jupyter-notebook-parser":{
            "from": "pypi",
            "config": {
            }
        }
    }
```

## `teedoc-plugin-search`: Website search plugin

Let the website and documents support the search function, support the searched current document and the whole site search

To use, add to the `plugins` keyword in `site_config.json`:
```json
"plugins": {
    "teedoc-plugin-search":{
        "from": "pypi",
        "config": {
            "search_hint": "Search"
        }
    }
}
```


The prompt content of different documents can be configured in the corresponding document `config.json` to facilitate multi-language support (internationalization/i18n)

The supported configurations are as follows:

```json
"teedoc-plugin-search":{
    "config": {
        "search_hint": "Search",
        "input_hint": "Enter keywords, separate multiple keywords with spaces",
        "loading_hint": "Loading, please wait...",
        "download_err_hint": "Failed to download the file, please refresh and try again or check the network",
        "other_docs_result_hint": "Results from other documents",
        "curr_doc_result_hint": "Current document search result"
    }
}
```

* `search_hint`: the prompt message of the search box (button), the default is `Search`
* `input_hint`: input hint information in the search box of the search page, default `Keywords separated by space`
* `loading_hint`: Load the file prompt for searching, the default is `Loading, wait please ...`
* `download_err_hint`: The download of the file required for the search fails, the user needs to refresh the browser to try again or the network environment cannot download the file, the default is `Download error, please check network and refresh again`
* `other_docs_result_hint`: search result hint, search result in other documents, default `Result from other docs`
* `curr_doc_result_hint`: search result hint, the search result in the currently browsed document, default `Result from current doc`


## `teedoc-plugin-baidu-tongji`: Baidu Analytics

Add the code of [Baidu Analytics](https://tongji.baidu.com/) on each page, send the visit information to Baidu, and you can see the visit statistics in the background

After registering and logging in to Baidu Statistics, add a website on the management page, and then there will be a code acquisition page with the following code
```js
<script>
var _hmt = _hmt || [];
(function() {
  var hm = document.createElement("script");
  hm.src = "https://hm.baidu.com/hm.js?90c693aa2************c14a50bb49";
  var s = document.getElementsByTagName("script")[0];
  s.parentNode.insertBefore(hm, s);
})();
</script>
```

Here is a string of keys `90c693aa2************c14a50bb49`, copy this string of keys, and then add to the `plugins` keyword in `site_config.json`:
```json
"plugins": {
    "teedoc-plugin-baidu-tongji":{
        "from": "pypi",
        "config": {
            "code": "Fill in the access key here"
        }
    }
}
```

After the website is deployed, you can see the visit information on the real-time visitor page of the Baidu statistics background


## `teedoc-plugin-gitalk`: gitalk comment plugin

[gitalk](https://github.com/gitalk/gitalk) is a comment tool based on the `github` `issue` question and answer system. Using `gitalk`, you don’t need to build your own server, you only need to register a `github` account , All the data will be put on the `issue` of a repository of `github`.

To use it, you need to add a plug-in in the `site_config` file
```json
    "teedoc-plugin-gitalk": {
        "from": "pypi",
        "config": {
            "classes": [
                "type_doc",
                "type_blog"
            ],
            "env": {
                "clientID": "********",
                "clientSecret": "********",
                "repo": "repo name",
                "owner": "orgnization name or user name",
                "admin": ["user names have write access"]
            }
        }
    }
```

* `classes`: Pages with these classes need to add comments, the default `type_doc` and `type_blog`, these two classes are automatically given to the document page and the blog page by the `<body> of the `teedoc-plugin-theme-default` `tag added
* `clientID` and `clientSecret`: You need to create a new application in [github application](https://github.com/settings/applications/new) to get `ID` and `Secret`
* `repo` and `owner`: are the name and owner of the repository, for example, [github.com/teedoc/teedoc.github.io](https://github.com/teedoc/teedoc.github.io) is used here `issue` as a comment system, just fill in `teedoc.github.io` and `teedoc`
* `admin`: is the name of the user who has write access to this repository

