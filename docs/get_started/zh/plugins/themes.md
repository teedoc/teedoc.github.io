---
title: teedoc 主题插件
keywords: teedoc, 主题插件, 主题, 插件
desc: teedoc 主题插件
---


## `teedoc-plugin-theme-default`: 默认主题插件

### 插件配置

在`site_config.json`中配置插件
```json
    "plugins": {
        "teedoc-plugin-theme-default":{
            "from": "pypi",
            "config": {
                "dark": true,
                "env":{
                    "main_color": "#4caf7d",
                    "sidebar_width": "300px"
                },
                "css": "/static/css/custom.css",
                "js": "/static/js/custom.js",
                "code_highlight_css":  "/static/css/prism.css",
                "code_highlight_js": "/static/js/prism.js"
            }
        }
    }
```

* `main_color`: 主题主颜色
* `sidebar_width`: 侧边目录栏默认宽度, 可以是`"300px"`, `300`, `"30%"` 这样的格式
* `css`: `css`文件 `URL`，可以覆盖默认的样式，会被插入到页面的`head`标签中
* `js`： `js`文件 `URL`，可以写`js`程序，会被放在页面的末尾加载

默认代码高亮使用了[prismjs](https://prismjs.com/), 默认勾选了部分常用语言的支持，`js`文件 + `css`文件共`100KiB`左右，如果你需要的代码无法高亮，或者想节约流量或缩减加载时间，可以到[这里](https://prismjs.com/download.html#themes=prism-tomorrow&languages=markup+css+clike+javascript+bash+c+cpp+cmake+coffeescript+docker+go+ini+java+json+json5+kotlin+latex+less+lua+makefile+markdown+markup-templating+objectivec+php+powershell+python+jsx+tsx+ruby+rust+sass+scss+shell-session+sql+swift+textile+typescript+yaml&plugins=line-numbers+highlight-keywords+toolbar+copy-to-clipboard+match-braces)查看默认勾选的以及勾选自己需要的语言和功能， 最后获得一个`css`文件和一个`js`文件放到`static/js/`目录下，然后需要在 `site_config.json` 中设置`URL`比如：
```json
    "route": {
        "assets": {
            "/static/": "static"
        }
    }
```
> 这会将`static`目录下所有文件拷贝到输出文件夹`static`目录下面

* `code_highlight_css`: `code`高亮`css`文件 `URL`，会取代默认的高亮`css`文件，会被插入到页面的`head`标签中
* `code_highlight_js`： `code`高亮`js`文件 `URL`，会取代默认的高亮`js`文件，会被放在页面的末尾加载

支持 `白天` 和 `夜间` 模式， 夜间模式会在`body`加一个`dark`类， 如果要该夜间模式的`css`样式，可以基于这个类名修改

### class 支持

主题支持几个常用的`class` 和 `id`， 可以在`config.json`(/`config.yaml`) 或者或者`.md`文件的`class`和`id`关键字中设置，就可以直接使用这个样式了

比如在`config.json`中设置
```json
{
    "class": "language_zh",
    "navbar": {
        ...
    }
}
```

则这个文档下面所有生成的页面的`html`标签都会包含这个类， 比如`language_zh`会让右边页面目录的标题从罗马数字变成中文`一、二、三...`

`class`有：
* `language_zh`： 会让右边页面目录的标题从罗马数字变成中文`一、二、三...`
* `md_page`: 普通 markdown 文件渲染成 html 页面，可以加上这个类，会居中显示而不是左对齐
