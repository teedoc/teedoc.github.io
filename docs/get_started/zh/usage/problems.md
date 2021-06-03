
---
title: 使用 teedoc 时的常见问题
keywords: teedoc, build, error, 出错, 问题, 无法运行
desc: 使用 teedoc 时的常见问题
---





## jinja2.exceptions.TemplateNotFound: base.html.j2

需要更新 `nbconvert`：

```shell
pip3 install --upgrade nbconvert
```

## 插件报错, 代码抛出错误

可以先尝试更新`teedoc`:

```
pip install -U teedoc
```

然后再在相应的文档下面更新插件:

```
teedoc install
```

如果仍然有问题, 可以到 github [提交 issue](https://github.com/teedoc/teedoc/issues/new)


