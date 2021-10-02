# git学习笔记

## git基础知识

### 概述

#### 仓库初始化
1. 初始化  
在工作目录下执行`git init`生成git仓库，之后工作目录下任何的更改都将被git追踪。
2. git配置  
- git的配置分为仓库级、用户级和系统级三个级别，配置文件分别为`.git/config`、`~/.gitconfig`、`git安装目录/etc/gitconfig`。  
- 执行`git config [--local/--global/--system] <section.key> <value>`设置各级配置项，例如`user.name`、`user.email`。  
- 执行`git config [--local/--global/--system] -l`查看各级配置信息。  

#### 本地仓库三区
为了使文件的更改更灵活，git采用了三区流转的方式，可以通过`git status`命令可以查看不同阶段的文件状态。  
1. 工作区workspace  
- 程序文件所在的目录。工作区中的文件的任何更改都会被git跟踪到，可以在`.gitignore`文件中规定哪些文件不必跟踪。  
- 更改后未暂存，想撤销更改回到上次提交后的状态，执行`git checkout -- <file-name>`。  
2. 暂存区stage/index  
- 文件更改后执行`git add .`命令便将更改暂存到暂存区。  
- 执行`git reset HEAD [<file-name>]`将暂存的更改撤销，工作区保留更改。  
3. 版本库repository  
- 执行`git commit -m "<message>"`命令将暂存区的更改提交到版本库，产生一次提交记录，暂存区重新变"干净"。  
- 执行`git reset --hard <commit-id>`命令使三个区域都回退到对应`commit-id`这次提交后的状态。  
![git三区示意图](./imgs/git-zone.png)

#### 分支
- 为了便于多人协作，git提供了分支功能。每一个团队成员都可以根据要实现的功能创建不同的分支进行独立开发，开发完成后再合并到主分支。  
- 执行`git branch <branch-name>`创建分支，创建完成后执行`git checkout <branch-name>`切换到刚创建的分支，也可以通过`git checkout -b <branch-name>`将以上两步一起执行。  
- 切换分支之前如果当前分支有更改未提交，可以执行`git stash [save "<save message>"]`将当前的更改存储，处理完其他分支的工作之后再执行`git stash pop [stash@{0,1,..}]`恢复工作区和暂存区。  
- 执行`git branchname [-r/-a]`可以查看本地、远程或所有分支，其中前面带`*`的是当前分支。  
- 每一个分支独立执行三区的操作，有独立的更改记录文件，所以特别适合协同开发。  
- 执行`git merge [--no-ff/--squash] <brach-name>`可以将`brach-name`分支合并到当前分支，不同的参数提供了不同的合并策略；如果合并之前执行`git rebase`可以保证合并之后当前分支不分叉。 
  - 默认合并方式fast-forward  
![默认合并方式fast-forward](./imgs/gitmerge-ff.gif)  
  - no-ff模式
![no-ff模式](./imgs/gitmerge-noff.gif) 
  - squash模式  
![squash模式](./imgs/gitmerge-squash.gif)  
  - rebase模式  
![rebase模式](./imgs/gitmerge-rebase1.png) 
![rebase模式](./imgs/gitmerge-rebase2.png)


#### 标签
- 标签可以理解为提交记录的别名，通过`git tag -a <tag-name> -m "<message>" commit-id`为某次提交记录添加标签。  
- 添加标签之后，许多命令中的`commit-id`可以用`tag-name`替换。  

#### 远程仓库
- 远程仓库与本地类似，只不过仓库和文件存放在远程服务器上。  
- 在github、gitee等网站创建远程仓库之后，执行`git remote add <origin-name> <origin-url>`命令建立本地仓库与远程仓库之间的联系。添加完成后可通过`git remote [-v]`查看远程仓库信息。  
- 执行`git clone [-b <branch-name>] <origin-url> [<workspace-name>]`可以将远程仓库`branch-name`分支的内容复制到本地的`workspace-name`目录下。  
- 执行`git pull [<origin-name> <remote-branch-name>:<local-branch-name>]`可以将远程仓库`remote-branch-name`分支拉到本地，并合并到本地仓库的`local-branch-name`分支。  
- 执行`git push [<origin-name> <local-branch-name>:<remote-branch-name>]`可以将本地仓库的`local-branch-name`推到远程，并合并到远程仓库的`remote-branch-name`分支。  

#### 版本指针
git的结构像是由提交记录组成的数据链，每一个提交记录像是链条上的一个节点，每一个节点都有对应的HASH值id，同时git也提供了以下几个指针方便使用。
- `HEAD`：无论切换到哪个分支，执行何种操作始终指向当前分支的最近一次提交节点。`HEAD^`表示上一个提交节点，`HEAD^^`表示上两个提交节点，`HEAD~5`表示前溯5个提交节点。
- `master`、`dev`：大部分情况下指向所在分支的最近一次提交节点，但当使用`git rebase [<start-point>] [<end-point>] --onto [<branch-name>]`将提交应用到分支时会处于HEAD游离状态，该指针仍指向指令执行前的节点。需要执行`git reset <end-point>`将该指针与HEAD保持一致。
- `tag-name`：指向绑定的提交节点。

#### 合并策略
- 合并冲突往往产生于多人协作时，子分支向主分支合并时发现主分支已经合并了其他子分支的更改，而且与当前子分支修改了同一个文件。此时，需要手动修改文件解决冲突，提交后重新合并。
- 多人协作模式下，先从远程主机拉取代码，在本地合并后再推送到远程。

### 推荐工作流程
1. 在github、gitee等网站创建仓库并初始化；  
2. 将远程仓库克隆到本地；  
3. 根据需求创建dev、feature、bug等分支，在分支上进行开发、暂存、提交、合并；  
4. 将本地分支推送到远程仓库对应分支。
![分支管理策略](./imgs/git-branchstrategy.png)

## git常用命令

### git config

#### 三级配置
系统级配置作用域最广，优先级最低。
1. 仓库级local
- 对当前仓库有效
- 配置文件为`工作目录/.git/config`
2. 用户级global
- 对该用户创建的所有仓库有效
- 配置文件为`~/.gitconfig`
3. 系统级system
- 对系统中的所有仓库有效
- 配置文件为`git安装目录/etc/gitconfig`

#### 命令格式
- `git config [--local/--global/--system] -l`查看各级配置信息
- `git config [--local/--global/--system] -e`打开编辑器编辑各级配置文件
- `git config [--local/--global/--system] [--add] <section.key> <value>`设置配置项的值
- `git config [--local/--global/--system] [--get] <section.key>`获取配置项的值
- `git config [--local/--global/--system] --unset <section.key>`删除配置项

#### 常用配置项
- `user.name`用户名
- `user.email`邮箱
- `core.editor`编辑器
- `merge.tool`比较合并工具
- `alias.<cmd>`命令别名，例如`git config --global alias.lg "log --color --graph --pretty=format:'%Cred%h%Creset -%C(yellow)%d%Creset %s %Cgreen(%cr) %C(bold blue)<%an>%Creset' --abbrev-commit"`可以优化日志显示的格式。

### git init
初始化git仓库，在当前工作区产生一个.git目录，将当前工作区变为git管理的仓库。

### git add
- `git add [-A] .`将工作区所有变化，包括修改、新增、删除均暂存
- `git add [-u] .`只暂存修改和删除操作，不暂存新增

### git commit
- `git commit -m "<message>"`将暂存区更改提交到版本库
- `git commit -am "<message>"`可以同时把未暂存的工作区修改直接提交到版本库，暂存区中没有更改记录

### git status
查看三区状态

### git diff
- `git diff [-- <file-name>/--stat]`某文件在工作区和暂存区的差异，如果文件名缺省则比较所有文件，`--stat`查看整体概况
- `git diff <commit-id> [-- <file-name>/--stat]`工作区和版本库的差异
- `git diff --cached <commit-id> [-- <file-name>/--stat]`暂存区和版本库的差异
- `git diff <commit-id-1> <commit-id-2> [-- <file-name>/--stat]`版本库两次提交记录之间的差异
- `git diff <branch-name-1> <branch-name-2> [-- <file-name>/--stat]`不同分支最近一次提交记录的差异

### git log
此命令用于显示每个分支的历史提交记录，最重要的作用是通过它查询提交记录对应的哈希值id。
1. 筛选显示信息
- `git log --author=<pattern>`筛选作者，可以用正则表达式
- `git log --commiter=<pattern>`筛选提交者，可以用正则表达式
- `git log --since/--after=<date> --until/--before=<date>`筛选时间，例如`git log --since="2020.01.01" --until="2021.10.01"`筛选2020.1.1~2021.10.1之间的提交记录
- `git log --grep=<pattern>`筛选提交信息，可以用正则表达式，例如`git log --grep="update"`筛选提交信息中有update这个单词的提交记录
- `git log -- <file-name>`筛选文件名，例如`git log -- hello.c`筛选涉及hello.c这个文件的提交记录
- `git log -S <string>/-G <pattern>`筛选更改内容，可以用正则表达式
- `git log --merges/--no-merges`筛选合并相关或非合并相关的提交记录
- `git log --merge`筛选合并冲突文件
2. 格式优化
- `git log --online`每条记录单行显示
- `git log --stat/-p`显示细节
- `git shortlog`按提交者分类显示
- `git log --decorate`显示分支名
- `git log --graph`显示提交历史的分支结构
3. 推荐设置
设置命令别名`git config --global alias.lg "log --color --graph --pretty=format:'%Cred%h%Creset -%C(yellow)%d%Creset %s %Cgreen(%cr) %C(bold blue)<%an>%Creset' --abbrev-commit"`，之后使用`git lg`命令。

### git reset
`git reset [--soft/--mixed/--hard] <commit-id> [<file-name>]`
- `commit-id`要回退到的版本
- `file-name`如果缺省，则作用于仓库中所有文件
- `--soft`仅版本库回退，暂存区和工作区不回退
- `--mixed`版本库、暂存区回退，工作区不回退
- `--mixed`版本库、暂存区、工作区都回退

### git reflog
如果之前通过`git reset --hard  <commit-id>`回退到旧版本。之后又想要复原成新版本，然而通过`git lg`已经查询不到新版本的哈希值了，此时可以执行`git reflog`命令查询。

### git rm
`git rm [--cached] <file-name>`
- `--cached`：只从暂存区删除
- 缺省：同时从工作区和暂存区删除

### git remote
- `git remote [-v]`列出已关联的远程仓库，加上`-v`可以同时列出远程仓库的url
- `git remote add <remote-repo-name> <remote-repo-url>`关联远程仓库
- `git remote rm <remote-repo-name>`取关远程仓库
- `git remote set-url <remote-repo-name> <remote-repo-url>`更新远程仓库的url
- `git remote rename <old-remote-repo-name> <new-remote-repo-name>`修改远程仓库别名

### git push
`git push [-u/--all/--force/--tags] <remote-repo-name> <local-branch-name>:<remote-branch-name>`
- 远程分支名缺省：将本地分支推送到远程仓库的同名分支
- 本地分支名缺省：删除远程分支
- 本地分支名和远程分支名都缺省：推送本地当前分支到远程仓库同名分支
- 远程主机名、本地分支名、远程分支名都缺省：在只有一个远程仓库时可以这样写，把本地当前分支推送到远程同名分支
- `-u`：首次推送时用于本地分支和远程分支的关联
- `--all`：git的推送策略有`simple`和`matching`两种，由`push.default`这个配置项决定，`simple`只推送当前分支，`matching`推送有对应关系的所有分支，`--all`则更进一步，没有对应分支也推送
- `--force`：如果本地版本低于远程版本，强制推送
- `--tags`：同时推送标签

### git clone
`git clone [-b <branch-name>] <remote-repo-url> [workspace-name]`
- `-b <branch-name>`：选择要克隆的分支，缺省默认为master分支
- `workspace-name`：克隆下来的仓库目录名，缺省则保持远程仓库中的名称

### git pull
- `git pull`相当于`git fetch`+`git merge`，从远程仓库分支拉取代码并合并到本地仓库分支。
- `git pull <remote-repo-name> <remote-branch-name>:<local-branch-name>`
  - `<remote-branch-name>`缺省默认为master
  - `<local-branch-name>`缺省默认为当前分支

### fit fetch
`git fetch <remote-repo-name>`获取远程仓库各分支提交记录存入本地仓库中的远程仓库状态副本，但并不合并到本地仓库。

### git checkout
- `git checkout [-b/-B] <branch-name>`
  - 无参数：切换到`<branch-name>`分支
  - `-b`：创建并切换到`<branch-name>`分支，如果`<branch-name>`分支已存在，报错
  - `-B`：创建并切换到`<branch-name>`分支，如果`<branch-name>`分支已存在，覆盖
- `git checkout [--orphan/--merge/-p] <branch-name> [<commit-id>]`
  - `--orphan`：基于当前分支创建新分支，新分支不保留当前分支的commit记录
  - `--merge` ：基于当前分支创建新分支，把当前分支未提交的修改内容也一起打包带走，当前分支未提交的修改内容丢失
  - `-p`：比较`<branch-name>`分支和当前分支的不同，提供交互式打补丁的选项
  - `<commit-id>`：如果缺省则以最近提交版本为基准，否则已`<commit-id>`对应的版本为基准
- `git checkout -- <file-name>`撤销工作区的更改，回到最近一次提交的状态

### git switch
- `git switch <branch-name>`切换分支
- `git switch -c <branch-name> [<commit-id>/<tag-name>]`以最近提交记录、某次提交记录、某个标签对应的提交记录创建分支，并切换到分支
- `git switch --orphan <branch-name>`创建一个不保留当前分支提交记录的新分支
- `git switch --detach <commit-id>`当前分支退回某个提交记录对应的状态

### git branch
- `git branch [-r/-a/-vv]`
  - 无参数：列出本地所有分支，当前分支前带`*`
  - `-r`：列出远程分支
  - `-a`：列出本地和远程所有分支
  - `-vv`：查看本地分支和远程分支的对应关系
- `git branch [-d/-D] <branch-name>`
  - 无参数：创建新分支
  - `-d`：删除分支
  - `-D`：强制删除分支
- `git branch -m <old-branch-name> <new-branch-name>`分支重命名
- `git branch --set-upstream-to=<remote-repo-name>/<remote-branch-name> <local-branch-name>`将本地分支与远程分支关联

### git merge
`git merge [--no-ff/--squash] <branch-name>`
- 无参数：采用默认的`fast-forward`策略合并将`<branch-name>`分支合并到当前分支上，`<branch-name>`分支上的提交记录会续在当前分支节点之后，不产生额外的节点，如果两个分支有分叉的情况，不可以使用这种策略合并。
- `--no-ff`：会在当前分支上新增一个额外的节点，这个节点涵盖了`<branch-name>`分支上所有的更改。
- `--squash`：类似`--no-ff`方式，区别在于当前分支不保留对`<branch-name>`分支分支的引用，在log中看不出合并操作。

### git stash
- `git stash save "<save-message>"`存储当前未提交的工作区、暂存区更改
- `git stash list`列出存储列表
- `git stash clear`删除所有的存储记录
- `git stash show/apply/pop/drop [stash@{<num>}]`
  - `show`：显示存储列表中某条记录对应的改动信息，如果缺省默认堆栈中的第一个
  - `apply`：还原某条记录对应的改动，但不在存储列表中删除该条记录
  - `pop`：还原改动的同时删除存储列表中该条记录
  - `drop`：删除存储列表中对应的记录，并不还原该记录对应的改动

### git cherry-pick
- `git cherry-pick [-n] [-m parent-number] <commit-id>/<branch-name>/<commit-id-1> <commit-id-3>/<commit-id-1>^..<commit-id-3>`
  - `-n`：只更新工作区和暂存区，不产生新的提交记录
  - `-m parent-number`：如果原始提交是合并节点，即有两个及以上的父节点，通过`parent-number`来选择使用哪个分支的改动(哪个父节点到原始节点的更改操作)
  - `<commit-id>`将这个提交记录对应的更改应用到当前分支
  - `<branch-name>`将这个分支的最近一次提交记录对应的更改应用到当前分支
  - `<commit-id-1> <commit-id-3>`将这两个提交记录对应的更改都应用到当前分支
  - `<commit-id-1>^..<commit-id-3>`将这两个提交记录之间的所有提交记录(前开后闭)对应的更改都应用到当前分支
- `git cherry-pick --continue/--abort/--quit`
  - `--continue`：`git cherry-pick`操作如果产生代码冲突，手动更改代码并`git add`提交到暂存区之后，此命令将恢复之前的`git cherry-pick`进程
  - `--abort`：`git cherry-pick`操作如果产生代码冲突，此命令放弃`git cherry-pick`进程，三区都回到操作前状态
  - `--quit`：`git cherry-pick`操作如果产生代码冲突，此命令退出`git cherry-pick`进程，三区保持当前状态

### git rebase
- `git rebase`：当前分支和待合并分支存在分叉时，此命令可以将待合并分支的基点改为当前分支的最新节点，然后再执行`git merge`命令将待合并分支合并到当前分支，这样可以使当前分支始终保持直线结构不分叉。
- `git rebase --continue`：如果`git rebase`更改基点产生冲突，则手动更改冲突代码，执行`git add .`暂存，然后执行此命令恢复`git rebase`进程。
- `git rebase -i <commit-id-1>^ <commit-id-10>`：进入交互界面，对(`<commit-id-1>^`,`<commit-id-10>`]区间内的提交记录执行以下操作，常用于将多个提交记录合并为一个。
  - `pick`简写为`p`，保留提交记录
  - `reword`简写为`r`，更改提交的备注信息
  - `squash`简写为`s`，将提交记录与前一个记录合并
  - `drop`简写为`d`，丢弃提交记录
  - 合并提交记录示例：
  ```
  p <commit-id-1>
  s <commit-id-2>
  ...
  s <commit-id-10>
  ```
- `git rebase <commit-id-1> <commit-id-10> --onto <branch-name>`
  - 将(`<commit-id-1>^`,`<commit-id-10>`]区间内的提交记录全部应用于`<branch-name>`分支。
  - 执行此操作后`HEAD`指向`<commit-id-10>`，`<branch-name>`指针不变，处于HEAD游离状态，须执行`git reset --hard <commit-id-10>`更新`<branch-name>`指针指向。

### git tag
- `git tag <tag-name>/-a <tag-name> <commit-id> [-m "<message>"]`
  - `<tag-name>`为当前分支最后一次提交记录打标签
  - `-a <tag-name> <commit-id>`为某次提交记录打标签
  - `-m "<message>"`添加备注信息
- `git tag -l`列出本地仓库所有标签
- `git ls-remote --tags <remote-repo-name>`列出远程仓库所有标签
- `git show <tag-name>`查看标签详情
- `git tag -d <tag-name>`删除本地仓库标签
- `git push <remote-repo-name> :refs/tags/<tag-name>`删除远程仓库标签，前提是本地仓库对应标签已删除
- `git push <remote-repo-name> --tags/<tag-name>`
  - `--tags`把本地所有标签推送到远程
  - `<tag-name>`把本地仓库中的`<tag-name>`标签推送到远程仓库
- `git checkout <tag-name>`切换到`<tag-name>`标签对应的提交节点