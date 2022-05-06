---
title: teedoc theme plugin
keywords: teedoc, theme plugin, themes, plugin
desc: teedoc theme plugin
---


## `teedoc-plugin-theme-default`: default theme plugin


### Plugin configuration

Configure the plugin in `site_config.json`
```json
    "plugins": {
        "teedoc-plugin-theme-default":{
            "from": "pypi",
            "config": {
                "dark": true,
                "default_dark": false,
                "mobile_navbar_collapsed": true,
                "toc_depth": 4,
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

* `dark`: support dark mode switch
* `default_dark`: default use dark mode
* `mobile_navbar_collapsed`: collapse navbar by default on mobile phone
* `toc_depth`: table of article conent's depth, default to `4`, that is show `H1~H4` headers
* `show_print_page`: show print page button, visitor click or push `Ctrl+P` to print
* `main_color`: theme main color
* `sidebar_width`: sidebar default width, format can be like`"300px"`, `300`, `"30%"`
* `css`: `css` file `URL`, which can override the default style and will be inserted into the `head` tag of the page
* `js`: `js` file `URL`, can write `js` program, it will be loaded at the end of the page

The default code highlighting uses [prismjs](https://prismjs.com/), and the support of some common languages ​​is checked by default. `js` file + `css` file totals about `100KiB`, if you need the code If you can’t highlight, or want to save traffic or reduce loading time, you can go to [here](https://prismjs.com/download.html#themes=prism-tomorrow&languages=markup+css+clike+javascript+bash+c+cpp+cmake+coffeescript+docker+go+ini+java+json+json5+kotlin+latex+less+lua+makefile+markdown+markup-templating+objectivec+php+powershell+python+jsx+tsx+ruby+rust+sass+scss+shell-session+sql+swift+textile+typescript+yaml&plugins=line-numbers+highlight-keywords+toolbar+copy-to-clipboard+match-braces) Check the default check and check the language and Function, finally get a `css` file and a `js` file in the `static/js` directory, and then set the `URL` in `site_config.json`:
```json
    "route": {
        "assets": {
            "/static/": "static"
        }
    }
```
> this config will due to files in `static` dir will be copied to `static` dir of out dir.

* `code_highlight_css`: `code` highlighting `css` file `URL`, will replace the default highlighting `css` file, and will be inserted into the `head` tag of the page
* `code_highlight_js`: `code` highlighting `js` file `URL`, will replace the default highlighting `js` file, and will be loaded at the end of the page

Supports `day` and `night` modes. The night mode will add a `dark` class to the `body`. If you want the `css` style of the night mode, you can modify it based on this class name

### class support

The theme supports several commonly used `class` and `id`, which can be set in `config.json`(/`config.yaml`) or the `class` and `id` keywords in the `.md` file. You can use this style directly

For example, set in `config.json`
```json
{
     "class": "md_page",
     "navbar": {
         ...
     }
}
```

Or add in the header of the `md` file
```markdown
---
title: title
class: heading_no_counter
---
```

Then the `html` tags of all generated pages under this document will contain this class. For example, `md_page` will make the content displayed in the center instead of left-aligned

`class` has:
* `md_page`: Ordinary markdown files are rendered into html pages, you can add this class, it will be displayed in the center instead of left-aligned
* `heading_no_counter`: Cancel automatic heading numbering
