teedoc plugin development
===========

The plug-in development of `teedoc` is very simple. At present, you only need to modify the template according to the template to realize the new function. If you know about `python` `html` `css` `js`, it will be easier.

The operating principle of the plugin is that `teedoc` provides [plugin API](https://github.com/teedoc/teedoc/blob/main/teedoc/plugin.py), write a `python` package, inherit this class, and rewrite The required method (`API`) is fine, the specific meaning of each method is detailed in the [plugin API](https://github.com/teedoc/teedoc/blob/main/teedoc/plugin.py) file Annotation

> If you find that the API is unreasonable, you can [submit issue](https://github.com/teedoc/teedoc/issues) to initiate a discussion and improve together~


## Reference template

You can refer to [default plugin](https://github.com/teedoc/teedoc/tree/main/plugins)


## Plug-in directory structure

Here is [teedoc-plugin-baidu-tongji](https://github.com/teedoc/teedoc/tree/main/plugins/teedoc-plugin-baidu-tongji) as an example

* Create a directory, the directory name is the same as the plug-in name, it is recommended to start with `teedoc-plugin-` for easy search
* Then create a `setup.py` file, this is the general configuration file of the `python` package, the most important thing in the file is to execute the `setuptools.setup()` function, the parameters mainly include the following several, more can be `python` official document search

```python
setup(
    name='teedoc-plugin-baidu-tongji',
    version="1.0.2",
    author='Neucrack',
    author_email='xxx@xxx.com',
    description='baidu tongji plugin for teedoc',
    long_description=long_description, # can read from README.md
    long_description_content_type="text/markdown",
    url='https://github.com/Neutree/teedoc',
    license='MIT',
    # See https://pypi.python.org/pypi?%3Aaction=list_classifiers
    classifiers=[

        # Indicate who your project is intended for
        'Intended Audience :: Developers',

        # Pick your license as you wish (should match "license" above)
        'License :: OSI Approved :: MIT License',

        # Specify the Python versions you support here. In particular, ensure
        # that you indicate whether you support Python 2, Python 3 or both.
        'Programming Language :: Python :: 3'
    ],

    keywords='teedoc baidu tongji',

    # List run-time dependencies here. These will be installed by pip when
    # your project is installed. For an analysis of "install_requires" vs pip's
    # requirements files see:
    # https://packaging.python.org/en/latest/requirements.html
    install_requires=install_requires,

    # List additional groups of dependencies here (e.g. development
    # dependencies). You can install these using the following syntax,
    # for example:
    # $ pip install -e .[dev,test]
    extras_require={
        #'dev': ['check-manifest'],
        #'test': ['coverage'],
    },

    # You can just specify the packages manually here if your project is
    # simple. Or you can use find_packages().
    packages=packages,

    # If there are data files included in your packages that need to be
    # installed, specify them here. If using Python 2.6 or less, then these
    # have to be included in MANIFEST.in as well.
    package_data={
        "teedoc_plugin_baidu_tongji": ['assets/*'],
    },

    # Although'package_data' is the preferred approach, in some case you may
    # need to place data files outside of your packages. See:
    # http://docs.python.org/3.4/distutils/setupscript.html#installing-additional-files # noqa
    # In this case,'data_file' will be installed into'<sys.prefix>/my_data'
    data_files=[
        ],

    # To provide executable scripts, use entry points in preference to the
    # "scripts" keyword. Entry points provide cross-platform support and allow
    # pip to create the appropriate form of executable for the target platform.
    entry_points={
        #'console_scripts': [
        # #'gui_scripts': [
        #'teedoc=teedoc-plugin-markdown-parser.main:main',
        # ],
    },
)
```

* Create package

You need to create another subfolder to store the source code. The file name is just to replace the minus sign `-` in the project name with an underscore `_`, because the package name in the `python` code requires that the minus sign cannot be used, for example, here is `teedoc_plugin_baidu_tongji`

Then create a new file `__init__.py`

* Edit `__init__.py`

To realize the function of Baidu statistics, it is actually necessary to add a script specified by Baidu statistics to the `<head></head>` tags of all pages, that is, the `<script></script>` tag. At the same time, from the document Get the statistical number (`code`) in the configuration, the code is as follows:

```python
import os, sys
from teedoc import Plugin_Base
from teedoc import Fake_Logger



class Plugin(Plugin_Base):
    name = "teedoc-plugin-baidu-tongji"
    desc = "baidu tongji support for teedoc"
    defautl_config = {
    }

    def on_init(self, config, doc_src_path, site_config, logger = None):
        '''
            @config a dict object
            @logger teedoc.logger.Logger object
        '''
        self.logger = Fake_Logger() if not logger else logger
        self.doc_src_path = doc_src_path
        self.site_config = site_config
        self.config = Plugin.defautl_config
        self.config.update(config)
        self.logger.i("-- plugin <{}> init".format(self.name))
        self.logger.i("-- plugin <{}> config: {}".format(self.name, self.config))
        
        # set site_root_url env value
        if not "code" in config:
            self.logger.e('can not find config["code"] in plugin {}'.format(self.name))
            return
        baidu_tongji_code ='''<script>
var _hmt = _hmt || [];
(function() {{
  var hm = document.createElement("script");
  hm.src = "https://hm.baidu.com/hm.js?{}";
  var s = document.getElementsByTagName("script")[0];
  s.parentNode.insertBefore(hm, s);
}})();
</script>'''.format(config["code"])
        self.html_header_items = [baidu_tongji_code]


    def on_add_html_header_items(self):
        return self.html_header_items
```

You can see that `class Plugin(Plugin_Base):` inherits the `Plugin_Base` class, and then rewrites the `on_init` and `on_add_html_header_items` methods. When the document is built by `teedoc`, these two functions will be called at the right time.

We obtained the Baidu statistics of the `code` from the `config` in the `on_init` initialization function, then generated the content of the `<script>` tag and stored it in the `html_header_items`, and then returned it in the `on_add_html_header_items` function , So that `teedoc` will add this tag to the `<head>` tag of all `HTML` pages

* resource

Can be in `setup.py`
```python
    package_data={
        "teedoc_plugin_baidu_tongji": ['assets/*'],
    },
```
Specify the `assets` directory and all files under it as the package data file to copy to the installation path, and then you can use the relative path (`assets/`) to access in `__init__.py`, which can be accessed in `on_copy_files of `API` Copy this file to the output directory in `, you can see the method of [teedoc_plugin_search](https://github.com/teedoc/teedoc/blob/main/plugins/teedoc-plugin-search/teedoc_plugin_search/__init__.py), in fact Is returning a dictionary
```python
self.assets_abs_path = os.path.join(), "assets")
{
    "/static/js/search/search_main.js": os.path.join(os.path.dirname(os.path.abspath(__file__), "assets", "search_main.js")
}
```
The keyword is the target path to be copied to, and the value is the absolute path of the file, and then we can refer to it in other `HTML` pages through the path `/static/js/search/search_main.js`

## Test plugin

Two methods,

### One is to directly let `teedoc` call the source code

It is recommended to use this method for debugging. Modifying the code only needs to re-execute `teedoc serve` to take effect

* Modify a copy of the `site_config.json` file in the document root directory, and specify the source of the plug-in as the directory path
```json
"plugins": {
        "teedoc-plugin-baidu-tongji":{
            "from": "Path",
            "config": {
                "code": "9cb07365532534256c346c838181a"
            }
        }
    },
```

Then execute `teedoc serve` in the document root directory.


### Another way is to install the plug-in directly to the system and let `teedoc` call the package

Using this method, you need to go through the following steps every time you change the code to modify the code, which is more troublesome than the above method. You can use this method to test the usability before releasing the plug-in.

* Install plugin

After the plug-in is written, execute it in the plug-in root directory (the directory with the `setpu.py` file)
```shell
pip install.
```
Pay attention to the `.` symbol not to be ignored

The plugin will be installed to the system as a `python` package

* Then execute `teedoc serve` in the document root directory.


## Pay attention

Because the build will use multiple processes, so there are some places to pay attention to

* Plug-in initialization: `__init__()` function **cannot** be overridden, plug-in initialization can use `on_init()` or `on_new_process_init()`;
  * `on_init()` is called when the plugin is initialized, and general data can be initialized here. When multiple processes are created, the data of the plug-in will be copied to the new process for use. For some objects that cannot be directly copied by multiple processes, please initialize in `on_new_process_init()`
  * `on_new_process_init()` is called when multiple processes are created, such as [here](https://github.com/teedoc/teedoc/blob/main/plugins/teedoc-plugin-markdown-parser/teedoc_plugin_markdown_parser/__init__.py ) In this function to initialize the `markdown` renderer instead of in the `on_init()`, because you don’t want to copy the `self.md_parser` object when a new process is created, but each new process is rebuilt independently Create an object
* The same `__del__()` function cannot be used, but use `on_del()` or `on_new_process_del()` function
* In [plugin.py](https://github.com/teedoc/teedoc/blob/main/teedoc/plugin.py) the functions behind `on_new_process_init()` may be in a new process (multi-process) Called, while the previous function will only be called in the main process


## Publish plugin

The release method of the plug-in is the release method of the ordinary `python` package, published to [pypi.org](https://pypi.org/)

* Execute the command in the root directory of the plug-in to generate the release package:

```shell
python setup.py bdist_wheel sdist
```

* Register an account at [pypi.org](https://pypi.org/)

* Then upload to `pypi.org`

```shell
twine upload dist/*
```

* Then users can directly fill in your plugin name in `site_config.json` to use

It can also be installed via `pip`:

```shell
pip install your plugin name
```


