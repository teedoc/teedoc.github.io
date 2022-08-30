---
title: list all files in order of file size/find large files
Keywords: find large files in a directory, sort by file size
desc: Use the teedoc tool teedoc-list-files command to list all files according to the file size in the directory, which is convenient for finding large files
---

Use the `teedoc` tool `teedoc-list-files` command to list all files according to the file size in the directory, which is convenient for finding large files

Using help:
````
teedoc-list-files -h
````

For example, to list all the files in the directory `my_doc` and sort them by file size, you can use this:

````
teedoc-list-files -r my_doc all
````

Here `-r` is a recursive search, that is, it will search for subfolders, `all` represents all file formats, and other formats can also be specified, such as:

````
teedoc-list-files -r my_doc jpg
````

You will see the sorted list of files.
````
402.1 KiB - 411708 : get_started/assets/images/jupyterlab.jpg
234.2 KiB - 239864 : get_started/assets/images/pages_settings.jpg
215.7 KiB - 220828 : get_started/assets/images/action_error.jpg
165.8 KiB - 169815 : get_started/assets/images/action_error_log.jpg
139.6 KiB - 142970 : get_started/assets/images/jupyterlab_1.jpg
````


If you want to remove file totally from git repo, refer to [github doc](https://docs.github.com/en/authentication/keeping-your-account-and-data-secure/removing-sensitive-data-from-a-repository) or refer to [gitee doc](https://gitee.com/help/articles/4232#article-header2)


