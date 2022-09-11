---
title: 对比两个目录中的不同文件和改动文件
keywords: 对比两个目录, 找出文件更改
---

使用 `teedoc` 中的工具 `teedoc-compare` 命令来比较两个目录文件的不同。
比如我们在生成新的 `out` 目录后，需要和旧的 `out` 目录对比，来找出发生了更改的文件，这样我们可以只上传改动了的文件。


比如旧的生成 html 的目录为`old_out`，新的为`out`，则通过以下命令对比
```bash
teedoc-compare old_out out
```

会用 `json` 格式打印出不同的文件:
```json
{
    "new": [],
    "modified": [],
    "deleted": []
}
```

使用帮助：
```
teedoc-compare -h
```

