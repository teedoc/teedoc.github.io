teedoc 插件开发
===========

`teedoc` 的插件开发很简单， 目前只需要根据模板修改一下就可以实现新的功能， 如果会 `python` `html` `css` `js` 则会更加简单。

插件的运行原理就是`teedoc` 提供[插件 API](https://github.com/teedoc/teedoc/blob/main/teedoc/plugin.py)， 写一个`python`包， 继承这个类， 重写需要的方法（`API`）即可， 具体每个方法的含义在[插件 API](https://github.com/teedoc/teedoc/blob/main/teedoc/plugin.py)文件中有详细的注释说明

> 如果发现 API 有不合理的地方， 可以[提交 issue](https://github.com/teedoc/teedoc/issues) 来发起讨论， 一起完善~


## 参考模板

可以参考[默认的插件](https://github.com/teedoc/teedoc/tree/main/plugins)


## 插件目录结构

这里以[teedoc-plugin-baidu-tongji](https://github.com/teedoc/teedoc/tree/main/plugins/teedoc-plugin-baidu-tongji) 为例

* 创建一个目录， 目录名和插件名相同， 建议以`teedoc-plugin-`开头，方便大家搜索到
* 然后创建一个`setup.py`文件， 这个是`python`包的通用配置文件， 文件中最重要的就是执行`setuptools.setup()`函数， 参数主要包含了一下几个，更多可以到`python`官方文档查找

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

    # List run-time dependencies here.  These will be installed by pip when
    # your project is installed. For an analysis of "install_requires" vs pip's
    # requirements files see:
    # https://packaging.python.org/en/latest/requirements.html
    install_requires=install_requires,

    # List additional groups of dependencies here (e.g. development
    # dependencies). You can install these using the following syntax,
    # for example:
    # $ pip install -e .[dev,test]
    extras_require={
        # 'dev': ['check-manifest'],
        # 'test': ['coverage'],
    },

    # You can just specify the packages manually here if your project is
    # simple. Or you can use find_packages().
    packages=packages,

    # If there are data files included in your packages that need to be
    # installed, specify them here.  If using Python 2.6 or less, then these
    # have to be included in MANIFEST.in as well.
    package_data={
        "teedoc_plugin_baidu_tongji" : ['assets/*'],
    },

    # Although 'package_data' is the preferred approach, in some case you may
    # need to place data files outside of your packages. See:
    # http://docs.python.org/3.4/distutils/setupscript.html#installing-additional-files # noqa
    # In this case, 'data_file' will be installed into '<sys.prefix>/my_data'
    data_files=[
        ],

    # To provide executable scripts, use entry points in preference to the
    # "scripts" keyword. Entry points provide cross-platform support and allow
    # pip to create the appropriate form of executable for the target platform.
    entry_points={
        # 'console_scripts': [
        # # 'gui_scripts': [
        #     'teedoc=teedoc-plugin-markdown-parser.main:main',
        # ],
    },
)
```

* 创建包

需要再创建一个子文件夹，用来存放源码，文件名将项目名称中的减号`-`替换成下划线`_`即可， 因为`python`代码中要求包名不能用减号， 比如这里是`teedoc_plugin_baidu_tongji`

然后在里面新建一个文件`__init__.py`

* 编辑`__init__.py`

要实现百度统计的功能， 实际就是向所有页面的`<head></head>`标签中添加一段百度统计指定的脚本即可，即`<script></script>`标签， 同时， 从文档配置中获取统计编号（`code`）， 代码如下：

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
        baidu_tongji_code = '''<script>
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

可以看到`class Plugin(Plugin_Base):`继承了`Plugin_Base`类， 然后重写了`on_init`和`on_add_html_header_items`方法， `teedoc`构建文档时，会在合适的时机调用这两个函数。

我们在`on_init`初始化函数中从`config`中获取到了百度统计的`code`，然后生成了`<script>`标签的内容存放到了`html_header_items`中， 然后在`on_add_html_header_items`函数中将其返回，这样`teedoc`就会将这个标签添加到所有`HTML`页面的`<head>`标签中了

* 资源文件

可以在`setup.py`中
```python
    package_data={
        "teedoc_plugin_baidu_tongji" : ['assets/*'],
    },
```
指定将`assets`目录及其下所有文件作为包数据文件拷贝到安装路径， 然后就可以在`__init__.py`中使用相对路径(`assets/`)访问到了，可以在`API`的 `on_copy_files`中将这个文件拷贝到输出目录， 可以看[teedoc_plugin_search](https://github.com/teedoc/teedoc/blob/main/plugins/teedoc-plugin-search/teedoc_plugin_search/__init__.py)的做法， 其实就是返回了一个字典
```python
self.assets_abs_path = os.path.join(), "assets")
{
    "/static/js/search/search_main.js": os.path.join(os.path.dirname(os.path.abspath(__file__), "assets", "search_main.js")
}
```
关键字是要拷贝到的目标路径，值是文件的绝对路径， 然后我们就可以在其它`HTML`页面中通过`/static/js/search/search_main.js`这个路径引用到了

## 测试插件

两种方法，

### 一种是直接让`teedoc`调用源码

调试建议使用这种方法， 修改代码只需要重新执行`teedoc serve`就可以生效

* 修改一份文档根目录的`site_config.json`文件，指定插件的来源为目录路径
```json
"plugins": {
        "teedoc-plugin-baidu-tongji":{
            "from": "路径",
            "config": {
                "code": "9cb07365532534256c346c838181a"
            }
        }
    },
```

然后在文档根目录执行`teedoc serve`就可以了


### 另一种方法是直接安装插件到系统，让`teedoc`调用软件包

使用这种方法， 修改代码需要每次更改代码都要来一遍下面的步骤，比上面一种方法麻烦，在发布插件前可以用这种方式测试一下可用性即可

* 安装插件

插件写好了通过在插件根目录（有`setpu.py`文件的目录）下执行
```shell
pip install .
```
注意`.`符号不要忽略

插件就会作为一个`python`包被安装到系统

* 然后在文档根目录执行`teedoc serve`就可以了


## 注意点

因为构建是会用到多进程， 所以有些地方需要注意

* 插件初始化： `__init__()`函数**不能**重写，插件的初始化可以使用`on_init()`或者`on_new_process_init()`；
  * `on_init()`是在初始化插件时调用，一般的数据可以在这里面初始化。当多进程创建时，插件的数据会被拷贝到新的进程使用， 对于一些不能多进程直接拷贝使用的对象，请在`on_new_process_init()`中初始化
  * `on_new_process_init()`是在创建多进程时调用， 比如[这里](https://github.com/teedoc/teedoc/blob/main/plugins/teedoc-plugin-markdown-parser/teedoc_plugin_markdown_parser/__init__.py)就在这个函数里面来初始化`markdown`渲染器而不是在`on_init()`中，因为不希望在新进程创建时对`self.md_parser`对象进行拷贝，而是每个新进程都独立重新创建一个对象
* 同样`__del__()`函数也不能使用， 而是使用`on_del()`或者`on_new_process_del()`函数
* 在[plugin.py](https://github.com/teedoc/teedoc/blob/main/teedoc/plugin.py)中`on_new_process_init()`后面的函数都是可能会在新进程（多进程）中调用的，前面的函数则只会在主进程中调用



## 发布插件

插件的发布方式就是普通`python`包的发布方式， 发布到[pypi.org](https://pypi.org/)

* 在插件根目录执行命令生成发布包：

```shell
python setup.py bdist_wheel sdist
```

* 在[pypi.org](https://pypi.org/)注册账号

* 然后上传到`pypi.org`

```shell
twine upload dist/*
```

* 然后用户就可以在`site_config.json`中直接填写你的插件名来使用了

也可以通过`pip`安装：

```shell
pip install 你的插件名
```

