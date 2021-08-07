layout template - customize HTML layout
==========

## How to use the layout template

First write the layout template file, support the `Jinja2` grammar, the writing method can refer to the default [writing of the layout template of the theme plugin](https://github.com/teedoc/teedoc/tree/main/plugins/teedoc-plugin-theme- default/teedoc_plugin_theme_default/templates).

The template file can be placed in two places, one is placed in the theme plug-in, the plug-in author can do this;

The other is to put it in the layout template folder (the default document root directory `layout`), and then use a specific layout template for a specific page separately, and add the `layout` key value at the article header `metadata`:
`layout`: The layout template used by the page. This key value is not required by default. The configuration in the theme plugin will be used. You need to customize the layout of this page. You can set this parameter. The path is relative to the `layout_root_dir in `site_config` `Path, `layout_root_dir` defaults to `layout`, so to use `layout/special_layout.html` only need to fill in `special_layout.html`


Here is a list of all supported variables:

## jinja2 template vars

### page head info

* `page_id`: page id , a string, set in doc config and doc metadata
* `page_classes`: page classes, a list, set in doc config and doc metadata
* `keywords`: keywords, a list, set in doc metadata
* `description`: description, a string, set in doc metadata
* `header_items`: tags in head(`<head></head>`) tag, a string list
* `title`: page title, can be null, string, set in doc metadata
* `site_name`: site name, string, set in site config

### navbar

* `logo_url`: site logo image url, string, can be null, set in doc config
* `logo_alt`: site logo alt info, string, can be null, set in doc config
* `home_url`: home url, e.g. "/", "/site0/", string
* `navbar_title`: navbar title, html string, can be null, set in doc config
* `navbar_main`: navbar left html, html string
* `navbar_options`: navbar right html, html string
* `navbar_plugins`: navbar plugins html, html string

### sidebar info

pages no sidebar, only article of docs have

* `sidebar_title`: sidebar title, string, can be html string, set in doc sidebar config
* `sidebar_items_html`: sidebar items html, html string

### body info

#### article of docs body info

* `article_title`: doc's article title, string, set in doc metadata
* `tags`: article tags, a string list, set in doc metadata
* `author`: article author, string, can be null, set in doc metadata
* `date`: article create date, string, can be null, set in doc metadata
* `show_source`: show source button info, string, can be null, set in site config and doc metadata
* `source_url`: source file's url, valid only when show_source is not null
* `body`: body html, html string
* `previous`: previous article, can be null
  * `title`: previous article title, string
  * `url`:  previous article url, string
* `next`: next article
  * `title`: next article title, string
  * `url`:  next article url, string
* `toc`: toc html string, can be null


#### pages body info

* `body`: body html, html string

### page footer info

* `footer_top`: footer top html string
* `footer_bottom`: footer bottom html string
* `footer_js_items`: js items last load int html, string list
