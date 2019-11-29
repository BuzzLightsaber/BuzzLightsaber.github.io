---
layout: post
---

# Git学习笔记

## 一些要点

- Linus用C开发
- 分布式版本控制
- 图片、视频、word这些格式只能跟踪是否改动，无法跟踪具体的改动
- 不要使用win自带的txt ，用notepad ++，因为编码格式问题
- HEAD指向的版本就是当前版本。命令`git reset --hard commit_id`就可以随意指向
- stage（也叫index），即暂存区。
- Git管理的是修改，当你用`git add`命令后，在工作区的第一次修改被放入暂存区，准备提交，但是，在工作区的第二次修改并没有放入暂存区，所以，`git commit`只负责把暂存区的修改提交了，也就是第一次的修改被提交了，第二次的修改不会被提交。
- origin就是git默认的远程库的名字。
- master指向提交，HEAD指向master（即当前分支）
- 当创建新的分支时，git新建了个`dev`指针，指向`master`相同的提交，再把`HEAD`指向`dev`，就表示当前分支在`dev`上。
- 可以用 `.gitignore` 忽略某些文件
- git 可以用命令来简化命令长度

## 实用命令

### 通用命令

---

- `mkdir` 创建路径
- `cd` 进入路径
- `pwd` 显示当前目录
- `ls` 查看当前路径下的文件
- `ls -ah` 查看目录下是否有.git文件
- `vi file.txt` 如没有文件可以创建，如有文件则直接编辑。编辑文本后可以按esc并输入ZZ（或者:wq）保存退出编辑模式。
- `rm file.file` 在文件管理器中删除文件

### 基本

---

- `git config -global user.name(email) "example"` 设立用户名和邮箱。`-global`表示全局
- `git init` 设立仓库
- `git add file.file` 把文件放到仓库，运行后没有提示说明成功
- `git commit -m "blahblah"`  提交到仓库
- `git status` 掌握仓库的动态，是否发生改变等等。
- `git diff` 对比查看两次提交的区别。
- `git log` 查看每次commit 的状态
- `git log --pretty=oneline` 将log精简到一行

### 版本

---

- `git reset --hard HEAD^` 将版本回退到上个版本。HEAD^^则表示上两个版本
- `git reset --hard commit_id` 将版本指向到指定的版本。
- `git reflog` 记录你的每一次命令
- `git diff HEAD -- file.file` 查看工作组和版本库里最新版本的区别
- `git checkout -- file.file` 丢弃当前修改。命令中的`--`很重要，没有`--`，就变成了“切换到另一个分支”的命令，我们在后面的分支管理中会再次遇到`git checkout`命令
- `git reset HEAD file.file` 如果修改已经add，则可以用该条命令unstage。重新放回工作区。
- `git rm file.file` 在版本库中删除文件

### 远程

---

- `git remote add origin git@server-name:path/repo-name.git` 关联本地库到远程库
- `git remote remove origin` 撤销关联
- `git push -u origin master` 将库的master分支推送到origin远程库上。由于远程库是空的，我们第一次推送`master`分支时，加上了`-u`参数，Git不但会把本地的`master`分支内容推送的远程新的`master`分支，还会把本地的`master`分支和远程的`master`分支关联起来，在以后的推送或者拉取时就可以简化命令。
- `git push origin master` 以后就可以直接把本地master分支推送到github。<https://blog.csdn.net/jingtingfengguo/article/details/51892864>SSH 的问题
- `git clone git@server-name:path/repo-name.git` 克隆本地仓库
- `git pull` 拉取

### 分支

---

- `git checkout -b dev `    =   `git branch dev`（创建） + `git checkout dev`（切换）  创建并切换分支，`-b`参数相当于两条命令
- `git branch` 查看当前分支，带有*号为当前分支
- `git merge` 合并指定分支到当前分支
- `git branch -d dev` 删除分支 /  `git branch -D xxx` 强制删除没有被合并的分支
- `git merge --no-ff` 禁用fast  forward参数。如果要强制禁用`Fast forward`模式，Git就会在merge时生成一个新的commit
- `git stash` 将现场工作储藏起来。需要是已经`add`过的。
- `git stash apply` + `git stash drop` 恢复+删除
- `git stash pop` ↑以上两条命令的集合
- `git stash list` 查看当前暂存区的内容
- `git remote` 查看当前远程库的信息  加上参数`-v`可以查看更详细的信息

### 标签

---

- `git tag vxxx`  打上vxxx的标签
- `git tag  `查看所有标签，顺序按照字母顺序排列
- `git tag vxxx yyyy`   为了yyyy版本打上vxxx的版本号
- `git tag -a <tagname> -m "blablabla..."`可以指定标签信息；
- `git tag show vxxxx` 查看指定版本
- `git tag -d vxxx` 删除标签，标签都储存在本地，不过自动推送到远程。
- `git push origin vxxx` 将某个标签推送到远程
- `git push origin -tags` 一次性推送全部标签
- `git tag -d vxxx`   `git push origin :refs/tags/v0.9`先删除本地标签，再删除远程标签