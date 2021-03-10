---
title: teedoc installation and use
keywords: teedoc, markdown, jupyter notebook, html, document generation, alternative gitbook, website generation, static website, installation, use
desc: teedoc, which converts markdown or jupyter notbook into html static web pages, introduces the use of teedoc
---


## Install python3

Need to install `Python3` first (only support `Python3`)

For example, on `Ubuntu`:
```shell
sudo apt install python3 python3-pip git
```

`Windows` and `macOS` please go to [Official Website Download](https://www.python.org/downloads/)



## Install teedoc

Open the terminal and enter:

```shell
pip3 install teedoc
```

Later use the following command to update the software:
```shell
pip3 install teedoc --upgrade
```

> If your network uses `pypi.org` and the speed is very slow, you can choose other sources, such as Tsinghua tuna source: `pip3 install teedoc -i https://pypi.tuna.tsinghua.edu.cn/simple`

Now you can use the `teedoc` command in the terminal

If not, please check if the `Python` executable directory is not added to the environment variable `PATH`,
For example, it may be in `~/.local/bin`


## New Construction

Create an empty directory to store the document project

```shell
mkdir my_site
cd my_site
teedoc init
```

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

This will install the plugin according to the plugin settings of `plugins` in `site_config.sjon`

```shell
cd my_site
teedoc install
```

> The plug-in is also released in the form of `python` package, so this will download the corresponding plug-in from `pypi.org`. Similarly, other sources can also be used, such as Tsinghua tuna source: `teedoc -i https://pypi. tuna.tsinghua.edu.cn/simple install`

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


## Create a new warehouse based on the template warehouse on github


Visit [https://github.com/teedoc/teedoc.github.io](https://github.com/teedoc/teedoc.github.io)

Click `Use this template`

![github use template](../../assets/images/github_use_template.jpg)

Then fill in the new warehouse name and description, create a new warehouse of your own based on this warehouse, and now you have obtained the warehouse exactly the same as the official website of teedoc

Then use `git clone your warehouse address` to clone to the local 
