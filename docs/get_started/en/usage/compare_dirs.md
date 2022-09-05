---
title: Compare different and changed files in two directories
keywords: compare two directories, find file changes
---

Use the `teedoc` tool `teedoc-compare` command to compare the differences between two directory files.
For example, after we generate a new `out` directory, we need to compare it with the old `out` directory to find out the changed files, so that we can upload only the changed files.


For example, the old directory for generating html is `old_out`, and the new one is `out`, then compare with the following command
```bash
teedoc-compare old_out out
```

will print out different files in `json` format:
```json
{
     "new": [],
     "modified": [],
     "deleted": []
}
```

Using help:
```
teedoc-compare -h
```
