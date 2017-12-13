# git status将中文文件名转义显示，如何修复？（如：\344\270\255）

关闭git的路径转义即可。

```Bash
git config --global core.quotepath false
```

# 团队项目中，如何设置自己的忽略文件而不影响版本库(.gitignore)

如果你有一些自己专用的文件需要忽略，又不想修改版本库中公共的.gitignore，可以使用全局忽略文件。

```Bash
# 1. 创建自己的全局忽略文件，名字位置均任意
echo '.vscode' >~/gitignore.global

# 2. 在Git中指定该文件
git config --global core.excludesfile ~/gitignore.global
```
