# git学习笔记
## git工作流
### git架构
#### 三区
为了使文件的更改更灵活，git采用了三区流转的方式，可以通过`git status`命令可以查看不同阶段的文件状态。
1. 工作区workspace
程序文件所在的目录。工作区中的文件的任何更改都会被git跟踪到，可以在`.gitignore`文件中规定哪些文件不必跟踪。
更改后未暂存，想撤销更改回到上次提交后的状态，执行`git checkout -- filename`。
2. 暂存区stage/index
文件更改后执行`git add .`命令便将更改暂存到暂存区。
执行`git reset HEAD filename`
3. 版本库repository
执行`git commit -m "message"`命令将暂存区的更改提交到版本库，产生一次提交记录，暂存区重新变"干净"。

2. 分支branch

### 推荐工作流程
## git常用命令清单