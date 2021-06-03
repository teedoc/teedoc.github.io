
---
title: teedoc build problems
keywords: teedoc, build, error, problem
desc: Problems when use teedoc
---



## jinja2.exceptions.TemplateNotFound: base.html.j2

Need upgrade `nbconvert`：

```shell
pip3 install --upgrade nbconvert
```


## Plugin error, or code raise error

Firstly, try to upgrade `teedoc` by:

```
pip install -U teedoc
```

Then update plugins in your doc directory:

```
teedoc install
```

If problems remain exists, [create issue](https://github.com/teedoc/teedoc/issues/new) on github

