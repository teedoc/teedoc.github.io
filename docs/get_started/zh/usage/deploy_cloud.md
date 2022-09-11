---
title: 部署 teedoc 生成的网站到云服务商的 Web 服务
keywords: teedoc, 部署, 云服务商, 云服务器, web服务
desc: 部署 teedoc 生成的网站到云服务商的 Web 服务
---


自建服务器需要自己购买服务器，需要配置服务器等等，没那么困难，但是稍微有点点复杂，另外一种选择就是使用云服务商提供的 Web 服务。

我们要做的就是将生成的 html 页面文件上传到云服务商的 Web 服务中，然后就可以通过云服务商提供的域名访问了，只需要给服务商付费就可以了（一般是流量+请求次数费用）。

比如常见的有阿里云、腾讯云、AWS、Google Cloud 等等，比如阿里云提供了 OSS 储存服务，腾讯云提供了 COS 储存服务，而且为静态网站做了优化，比如不带`.html`的访问连接会自动跳转到带`.html`的页面。
> 注意有些提供商没有对静态页面做优化，可能有些特性没法设置，比如七牛云不能支持不带`html`结尾的链接，比如`/test`不能自动转到`/test.html`，以及默认没有打开访问`/`自动转到`/index.html`，以及`404.html`不能直接从储存桶里面读取，需要手工上传到七牛云的配置里面


这里以腾讯云为例：

## 创建一个储存桶

* 注册腾讯云账号，开通 COS 储存服务
* 创建一个储存桶，比如叫`teedoc`，会得到一个储存桶名字(`bucket name`)，比如`teedoc-1250000000`，以及选择服务器地址，比如广州的`region`就是`ap-guangzhou`
* 设置里面启用静态网站选项，设置默认首页为`index.html`，设置404页面为`404.html`，如果不需要 CDN 可以把强制 HTTPS 打开，这样用户访问永远都是访问的 HTTPS 站点
* 设置完静态站点后，会得到一个域名，比如`teedoc-1250000000.cos-website.ap-guangzhou.myqcloud.com`，这个域名就是你的网站域名，可以通过这个域名访问你的网站
* 如果需要自定义成自己的域名，则修改“自定义源站域名”即可，其实就是设置你的域名的 CNAME 记录指向这个域名，比如`teedoc.example.com`的 CNAME 记录指向`teedoc-1250000000.cos-website.ap-guangzhou.myqcloud.com`，设置好了就可以通过`teedoc.example.com`访问你的网站了
* 另外云服务商一般都配套了 CDN 服务，如果你的网站访问量小，可以不用开启 CDN，如果访问量大，可以开启 CDN，这样可以加速访问速度，但是会增加费用。
另外设置 CDN 需要注意的是上一步的“自定义源站域名”的地方就不要设置了，直接设置“自定义 CDN 加速域名”即可


## 上传网站文件

通过 `teedoc` 生成了网站文件，在`out`目录下， 注意一定要用`teedoc build`命令生成， 不要用`teedoc serve`命令生成！

然后上传到腾讯云：
```
teedoc-upload --progress raw --cloud tencent --region ... --bucket ... --secret_id ... --secret_key ... out
```
这里`...`填对应的信息， `secret_id`和`secret_key`点击控制台头像->选择访问管理->API 密钥管理->创建密钥，然后就可以看到了，可以考虑专门建立一个子账号，然后只给这个子账号 COS 的权限，这样可以保证一定程度上的安全性。

另外，如果在自动构建系统中没法打印进度条，添加参数`--progress raw`；以及秘钥最好隐藏起来，比如 github 的 action 就提供了`secret`的功能：
```
teedoc-upload --progress raw --cloud tencent --region ${{ secrets.TENCENT_REGION }} --bucket ${{ secrets.TENCENT_BUCKET }} --secret_id ${{ secrets.TENCENT_SECRET_ID }} --secret_key ${{ secrets.TENCENT_SECRET_KEY }} out
```

## 更新网站文件

第一次上传文件后，后续不需要每次都上传所有文件，只需要上传更新的文件即可，这样可以节省时间和流量，比如：
```
teedoc-upload --progress raw --cloud tencent --region ... --bucket ... --secret_id ... --secret_key ... old_out out
```
这里 `old_out`是上一次生成的网站文件，`out`是本次生成的网站文件，`teedoc-upload`会自动比较两个文件夹，然后只上传更新的文件。







