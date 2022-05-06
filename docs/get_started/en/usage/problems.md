
---
title: teedoc build problems
keywords: teedoc, build, error, problem
desc: Problems when use teedoc
---


You can view the problems and solutions already mentioned at [github issue](https://github.com/teedoc/teedoc/issues?q=), and you can also submit problems, and you are welcome to find problems, modify the code and submit PR

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


## OSError: [Errno 28] inotify watch limit reached

It only appears when using `teedoc serve`, that is, the number of files monitored has reached the upper limit set by the system.
It usually happens when a large number of files are detected by the software, and the common situations are as follows:
* An editor such as `vscode` opens a folder with a lot of files. Turning off this kind of software or opening fewer files (folders) will usually solve the problem, refer to [error ENOSPC](https://code.visualstudio.com/docs/setup/linux#_visual-studio-code-is-unable-to-watch-for-file-changes-in-this-large-workspace-error-enospc)
* There are indeed too many documents in the document project, reaching the upper limit of monitoring. This situation is generally rare. If it occurs, you need to set the upper limit of the system’s monitoring. Please search for the specific method.

