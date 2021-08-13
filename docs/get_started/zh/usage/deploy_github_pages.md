---
title: 部署 teedoc 生成的网站到 github pages
keywords: teedoc, 部署
desc: teedoc 生成的网站部署到 github pages 服务, 使用 github 的免费服务器部署网站
---


因为 `github` 提供免费的 `pages` 服务, 我们只需要将`html`页面提交到`gh-pages`分支, 然后在项目设置里面开启`pages`服务即可.

同时, 我们也将页面构建和推送做了自动化, 每次向仓库提交代码/文档, 都会触发`github action`自动构建并推送到`gh-pages`页面, 十分简单方便可靠!


## 克隆模板仓库

这里使用 teedoc 官网的源码作为模板, 访问 [https://github.com/teedoc/template](https://github.com/teedoc/template) 或者 [https://github.com/teedoc/teedoc.github.io](https://github.com/teedoc/teedoc.github.io)
点击 `Use this template`
![github use template](../../assets/images/github_use_template.jpg)
然后填写新的仓库名称（如果你不准备自定义域名，则仓库名字最好是`teedoc.github.io`将`teedoc`替换成你的用户名或者组织名）和描述，基于这个仓库新建一个你自己的仓库，现在你获得了和 `teedoc`官网一模一样的仓库了
> 会发现有一个`.github/workflows`的目录，这个就是`github action`的配置文件，我们利用这个功能来实现：
> 当我们更新仓库的源文件时， `github action`自动根据这个配置文件的命令构建生成静态网页，然后推送到这个仓库的`gh-pages`分支

> 其中`sync_code_to_gitee.yml` 和 `sync_pages_to_gitee.yml` 是用来将代码同步到 `gitee` 的, 可以将他们删除, 需要时请自行研究


## 自动触发构建

仓库每提交一次修改，会自动触发构建，构建完成后`gh-pages`分支就会出现生成的`html`页面, 可以在`Actions`栏看到进度和日志

如果是绿色的勾， 就代表成功了，否则就是失败了，可以点进去看构建日志报了什么错，一般都是前面的值填错了

![](../../assets/images/github_action.jpg)

## 设置 pages 服务

然后在设置里面找到 `pages` 服务设置，打开即可，然后访问对应的域名，可以勾选 `Enforce HTTPS`来使能`HTTPS`（推荐）
比如这里是`https://teedoc.github.io/`

>! 注意如果根路径不是`/`，而且没有自定义域名（自定义域名见后面的介绍），比如`https://teedoc.github.io/my_site/`， 则需要在`site_config.json`中配置`"site_root_url":"/my_site/"`

![](../../assets/images/github_pages.jpg)

## Pages 服务自定义域名

部署好后会有一个默认的域名，比如`teedoc.github.io`， 如果想使用自己的域名，比如`teedoc.neucrack.com`，即需要自定义域名。

在`site_config.json`文件中设置了网站根目录映射的文件目录，比如这里是`pages/index/zh`目录：
```json
        "pages": {
            "/": "pages/index/zh",
            "/en/": "pages/index/en"
        },
```

在这个目录新建一个文件，取名`CNAME`注意没有后缀，然后写入自定义的域名比如`teedoc.neucrack.com`保存文件，提交即可

> 你可能在设置里面看到了`custom domain`的设置，但是需要像这里说的设置`CNAME`文件才能保持一直生效，不然下次提交自定义域名就失效了


## 自动构建进阶配置

在`.github/workflows/publish.yml` 文件中配置了构建的流程, 主要包含了构建文档, 和推送文档到`gh-pages`分支, 可以根据情况修改脚本.

推送文档到`gh-pages`分支用了, [github-pages-deploy-action](https://github.com/JamesIves/github-pages-deploy-action) 这个 `github actioin` 插件, 更多进阶的用法请阅读这个插件的文档

一般用上面的方式就足够了, 甚至不用配置任何参数就可以使用, 另外, `publish.yml` 底部提供了不用插件的推送写法, 如果你需要, 可以复制到上面使用, 使用时可能需要用到以下知识点:

* 生成 ssh 密钥

在本地生成一对`key`（windows下需要先安装相关软件，请自行安装（安装git就会有））
```
ssh-keygen -t rsa -f key.txt 
```
得到了`key.txt`和 `key.key.pub`两个文件

* 设置密钥

到文档项目的设置里面设置`ssh`密钥, 添加一个`ssh`密钥， 拷贝 `key.txt.pub` 中的内容，注意是`pub`文件，即公钥，选中**允许写入权限**, 名字随便取
![](../../assets/images/github_deploy_key.jpg)

然后点击`Secrets`，设置秘密键值对， 需要设置的变量有：
* `ACCESS_KEY`: 复制`key.txt`中的内容
* `GIT_EMAIL`: 设置一个`git`提交邮箱，设置你的`github`邮箱即可
* `GIT_NAME`: 设置一个`git`提交名字，设置你的`github`名字即可
* `REPO`: 仓库地址， 设置为`组织名/仓库名`或者`用户名/仓库名`，比如`teedoc/teedoc.github.io`即可
![](../../assets/images/github_secrets.jpg)

当然, 这些变量也可以通过优化脚本直接从提交记录中获得(除了`ACCESS_KEY`), 有兴趣可以优化, 也欢迎提交 PR


