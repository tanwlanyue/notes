---
title: Git 基础
date: 2024-03-21 23:12:54
categories: 工具
tags:
  - git
---

## Git常用命令速查表

```shell
#master 默认开发分支
#origin 默认远程版本库
#Head   默认开发分支
#Head^  Head的父提交

#创建版本库
git clone <url>                      #克隆远程版本库
git init                             #初始化本地版本库

#修改和提交
git status                           #查看状态
git diff                             #查看变更内容
git add .                            #跟踪所有改动过的文件
git add <file>                       #跟踪指定的文件
git mv <old> <new>                   #文件改名
git rm <file>                        #删除文件
git rm --cached <file>               #停止跟踪文件但不删除
git commit -m "commit message"       #提交所有更新过的文件
git commit --amend                   #修改最后一次提交

#查看提交历史
git log                              #查看提交历史
git log -p <file>                    #查看指定文件的提交历史
git blame <file>                     #以列表方式查看指定文件的提交历史

#撤消
git reset --hard HEAD                #撤消工作目录中所有未提交文件的修改内容
git checkout HEAD <file>             #撤消指定的未提交文件的修改内容
git revert <commit>                  #撤消指定的提交

#分支与标签
git branch                           #列出所有本地分支
git checkout <branch/tag>            #切换到指定分支或标签
git branch <new-branch>              #创建新分支
git branch -d <branch>               #删除本地分支
git tag                              #列出所有本地标签
git tag <tagname>                    #基于最新提交创建标签
git tag -d <tagname>                 #删除标签

#合并与衍合
git merge <branch>                   #合并指定分支到当前分支
git rebase <branch>                  #衍合指定分支到当前分支

#远程操作
git remote -v                        #查看远程版本库信息
git remote show <remote>             #查看指定远程版本库信息
git remote add <remote> <url>        #添加远程版本库
git fetch <remote>                   #从远程库获取代码
git pull <remote> <branch>           #下载代码及快速合并
git push <remote> <branch>           #上传代码及快速合并
git push <remote> :<branch/tag-name> #删除远程分支或标签
git push --tags                      #上传所有标签
```

## SSH KEY

```sh title="git配置" linenums="1"
# 生成 SSH Key
ssh-keygen -t rsa -C "tanwlanyue@gmail.com"
# 查看生成的 SSH 公钥
cat ~/.ssh/id_rsa.pub
# 测试
ssh -T git@gitee.com
```

## Config

> Win11设置系统默认编码格式为utf-8
>
> 1、打开**【设置】**菜单；
> 2、左侧点击**【时间和语言】**，右侧点击**【语言和区域】**；
> 3、相关设置下，点击**【管理语言设置】**；
> 4、区域窗口，管理选项卡下，点击**【更改系统区域设置】**；
> 5、区域设置窗口，勾选**【Beta 版：使用 Unicode UTF-8 提供全球语言支持(U)】**，然后点击**【确定】**；
> 6、更改系统区域设置后，还需要重启系统才可以生效；

```sh
# 用户设置
git config --global user.name "tanwlanyue"
git config --global user.email "tanwlanyue@gmail.com"
# ssl设置
git config --global http.sslVerify "false"
git config --global https.sslVerify "false"
# 解决Git中文乱码
git config --global core.quotepath false
git config --global gui.encoding utf-8
# 代理配置
git config --global http.proxy http://127.0.0.1:7890
git config --global https.proxy http://127.0.0.1:7890
```

## Project

```sh
# 解决vscode中git操作总是需要输入账号密码的问题
git config --global credential.helper store
```

<!-- more -->

## Command

```sh linenums="1" hl_lines="6 8"
# 删除分支
git branch -D branchName
# 修改最近的 commit message
git commit --amend
# 修改过去的 commit message
git rebase -i parentCommitId # (1)
# 合并连续的commit
git rebase -i parentCommitId # (2)
# 查看最近三次提交记录
git log -3
# 分离头指针保存
git branch newBranchName commitId 
git checkout -b newBranchName branch/commitId
# 比较差异
git diff HEAD HEAD~1
# 强制推送本地分支到远程分支
git push -f origin <branch>
# 强制拉取远程分支到本地分支
git fetch origin && git reset –hard origin/<branch>
# working directory->>stage/index
git add
# stage/index->>history
git commit
# history->>working directory
git checkout
# history->>garbage
git reset --hard
# history->>working directory
git reset (--mixed)
# history->>stage/index
git reset --soft
```

1. 选择需要修改的commit  前面的pick改为r
2. 选择需要修改的commit  前面的pick改为s

---

- `git clone <url>`：克隆远程版本库。
- `git init`：初始化本地版本库。
- `git status`：查看状态。
- `git diff`：查看变更内容。
- `git add <files>`：跟踪指定的文件。
- `git mv <old> <new>`：文件改名。
- `git rm <files>`：删除文件。
- `git rm --cached <files>`：停止跟踪文件但不删除。
- `git commit -m "commit message"`：提交所有更新过的文件。
- `git commit --amend`：修改最后一次提交。

查看提交历史

- `git log`：查看提交历史。
- `git log -p <files>`：查看指定文件的提交历史。
- `git blame <files>`：以列表方式查看指定文件的提交历史。

撤销

- `git reset --hard HEAD`：撤销工作目录中所有未提交的修改内容。
- `git checkout HEAD <files>`：撤销指定的未提交文件的修改内容。
- `git revert <commit>`：撤销指定的提交。

分支与标签

- `git branch`：显示所有本地分支。
- `git checkout <branch/tag>`: 切换到指定分支或标签
- `git branch <new-branch>`：创建新分支。
- `git branch -d <branch>`：删除本地分支。
- `git tag`：列出所有本地标签。
- `git tag <tagname>`：基于最新提交创建标签。
- `git tag -d <tagname>`：删除标签。

合并与衍合

- `git merge <branch>`：合并指定分支到当前分支。
- `git rebase <branch>`：衍合指定分支到当前分支。

远程操作

- `git remote -v`：查看远程版本库信息。
- `git remote show <remote>`：查看指定远程版本库信息。
- `git remote add <remote> <url>`：添加远程版本库。
- `git fetch <remote>`：从远程库获取代码。
- `git pull <remote> <branch>`：下载代码及快速合并。
- `git push <remote> <branch>`：上传代码及快速合并。
- `git push <remote> :<branch/tag-name>`：删除远程分支或标签。
- `git push --tags`：上传所有标签。





Git 和其他版本控制系统的主要差别在于，Git 只关心文件数据的整体是否发生变化，而大多数其他系统则只关心文件内容的具体差异，每次记录有哪些文件作了更新，以及都更新了哪些行的什么内容。Git 并不保存这些前后变化的差异数据。

实际上，Git 更像是把变化的文件作快照后，记录在一个微型的文件系统中。每次提交更新时，它会纵览一遍所有文件的指纹信息并对文件作一快照，然后保存一个指向这次快照的索引。为提高性能，若文件没有变化，Git 不会再次保存，而只对上次保存的快照作一链接。

![snapshot](./assets/snapshot.png)

对于任何一个文件，在 Git 内都只有三种状态：已修改（changes）已暂存（staged changes）已提交（committed）。

由此我们看到 Git 管理项目时，文件流转的三个工作区域：working dir，staged area，local repo。

**工作环境配置**

Git 提供了一个叫做 `git config` 的工具，专门用来配置或读取相应的工作环境变量。而正是由这些环境变量，决定了 Git 在各个环节的具体工作方式和行为。这些变量可以存放在以下三个不同的地方：

*	`/etc/gitconfig` 文件：系统中对所有用户都普遍适用的配置。若使用 `git config` 时用 ` --system` 选项，读写的就是这个文件。window在安装目录的/etc/gitconfig
*	`~/.gitconfig` 文件：用户目录下的配置文件只适用于该用户。若使用 `git config` 时用 ` --global` 选项，读写的就是这个文件。$USER/.gitconfig
*	当前项目的 `.git/config` 文件：只针对当前项目有效。每一个级别的配置都会覆盖上层的相同配置，所以 `.git/config` 里的配置会覆盖 `/etc/gitconfig` 中的同名变量。重复的环境变量取最后一个

```shell
# 用户信息
git config --global user.name "tanwlanyue"
git config --global user.email tanwlanyue@gmail.com
# 文本编辑器
git config --global core.editor vim
# 查看配置信息
git config --list
# 查看特定配置
git config user.name
# 获取帮助 学习 verb 命令可以怎么用
git help <verb>
git <verb> --help
man git-<verb>
# 解决合并冲突时使用哪种差异分析工具, Git可以理解kdiff3，tkdiff，meld，xxdiff，emerge，vimdiff，gvimdiff，ecmerge，opendiff 等的输出信息
git config --global merge.tool vimdiff
```

```shell
# 查看提交历史# 代码仓
git init
git clone <url> <name>
# 检查当前文件状态
git status
# 跟踪新文件
git add
# 比较的是工作目录中当前文件和暂存区域快照之间的差异
git diff
# 比较已经暂存起来的文件和上次提交时的快照之间的差异
git diff --staged
# 提交更新
git commit
# 从跟踪清单中删除
git rm --cached readme.txt
# 查看提交历史  最近的更新排在最上面
git log
# 常用 -p 选项展开显示每次提交的内容差异，用 -2 则仅显示最近的两次更新， --stat，仅显示简要的增改行数统计
git log -p -2 --stat
# 用 `oneline` 将每个提交放在一行显示
git log --oneline
# 定制要显示的记录格式
git log --pretty=format:"%h - %an, %ar : %s"
# 列出所有最近两周内的提交
git log --since=2.weeks
# 修改最后一次提交
git commit --amend
# 取消已经暂存的文件
git reset HEAD <file>
```

```shell
git log
-(n)	仅显示最近的 n 条提交
--since, --after	仅显示指定时间之后的提交。
--until, --before	仅显示指定时间之前的提交。
--author	仅显示指定作者相关的提交。
--committer	仅显示指定提交者相关的提交。
-- <path> path目录下的提交
--grep  搜索提交说明中的关键字
--all-match  筛选同时满足的提交
```

图形化工具查阅提交历史 gitk 

```shell
## 远程仓库的使用 ##
# 查看当前的远程库
git remote -v
# 添加远程仓库
git remote add <name> <url>
# 从远程仓库抓取数据 只拉取不会合并代码
git fetch <repo-name>
# 推送数据到远程仓库
git push <repo-name> <branch-name>
# 查看远程仓库信息
git remote show <repo-name>
# 远程仓库重命名
git remote rename <old> <new>
# 远程仓库的删除
git remote rm <repo-name>
```

```shell
## 标签 ##
# 列显已有的标签
git tag
# 特定的搜索模式列出符合条件的标签
git tag -l 'v1.4.2.*'
# 新建含附注的标签  -a 指定标签名字  -m 则指定了对应的标签说明
git tag -a v1.4 -m 'my version 1.4'
# 查看相应标签的版本信息
git show <tag-name>
# 新建轻量级标签
git tag <tag-name>
# 历史提交打标签
git tag -a <tag-name> <commitid>
# 分享标签
git push origin <tag-name>
# 一次推送所有本地新增的标签
git push origin --tags
```



```shell
# Git命令别名
git config --global alias.unstage 'reset HEAD --'
git config --global alias.last 'log -1 HEAD'
```

Git 的分支非常轻量级，它的新建操作几乎可以在瞬间完成，并且在不同分支间切换起来也差不多一样快。Git 保存的不是文件差异或者变化量，而只是一系列文件快照。

现在，Git 仓库中有五个对象：三个表示文件快照内容的 blob 对象；一个记录着目录树内容及其中各个文件对应 blob 对象索引的 tree 对象；以及一个包含指向 tree 对象（根目录）的索引和其他提交信息元数据的 commit 对象。

![单个提交对象在仓库中的数据结构](.\assets\单个提交对象在仓库中的数据结构.png)

作些修改后再次提交，那么这次的提交对象会包含一个指向上次提交对象的指针。

![多个提交对象之间的链接关系](.\assets\多个提交对象之间的链接关系.png)

现在来谈分支。Git 中的分支，其实本质上仅仅是个指向 commit 对象的可变指针。那么，Git 是如何知道你当前在哪个分支上工作的呢？其实答案也很简单，它保存着一个名为 HEAD 的特别指针。它是一个指向你正在工作中的本地分支的指针，每次提交后 HEAD 随着分支一起向前移动

```shell
# 创建分支 会在当前 commit 对象上新建一个分支指针
git branch <branch-name>
# 切换到其他分支 HEAD 就指向了目标分支
git checkout <branch-name>
# 要新建并切换到该分支
git checkout -b 
# 删除分支
git branch -d <branch-name>
# 列出本地所有分支
git branch
# 查看各个分支最后一个提交对象的信息
git branch -v
# 查看哪些分支是当前分支的直接上游
git branch --merged
# 显示还未合并进来的分支，git branch -d会报错丢失数据， git branch -D 强制执行
git branch --no-merged
# 推送本地分支
git push <repo-name> <branch-name>
# 上传我本地的 serverfix 分支到远程仓库中去，仍旧称它为 serverfix 分支
git push origin refs/heads/serverfix:refs/heads/serverfix
# 可以把本地分支推送到某个命名不同的远程分支
git push origin serverfix:awesomebranch
# 远程分支的内容合并到当前分支
git merge origin/serverfix
# 删除远程分支
git push origin :serverfix
```

rebase是按照每行的修改次序重放一遍修改，而merge并是把最终结果合在一起。

```
ssh-keygen
cat ~/.ssh/id_rsa.pub
```

```
# 看上一次提交
git show HEAD^

git reflog
#重写历史
git commit --amend
git rebase -i HEAD~3
```

Git可以在你提交时自动地把行结束符CRLF转换成LF，而在签出代码时把LF转换成CRLF。用`core.autocrlf`来打开此项功能，如果是在Windows系统上，把它设置成`true`，这样当签出代码时，LF会被转换成CRLF：

	$ git config --global core.autocrlf true

Linux或Mac系统使用LF作为行结束符，因此你不想 Git 在签出文件时进行自动的转换；当一个以CRLF为行结束符的文件不小心被引入时你肯定想进行修正，把`core.autocrlf`设置成input来告诉 Git 在提交时把CRLF转换成LF，签出时不转换。这样会在Windows系统上的签出文件中保留CRLF，会在Mac和Linux系统上，包括仓库中保留LF。

	$ git config --global core.autocrlf input



当你在一个新目录或已有目录内执行 `git init` 时，Git 会创建一个 `.git` 目录，几乎所有 Git 存储和操作的内容都位于该目录下。如果你要备份或复制一个库，基本上将这一目录拷贝至其他地方就可以了。本章基本上都讨论该目录下的内容。该目录结构如下：

	$ ls
	HEAD
	branches/
	config
	description
	hooks/
	index
	info/
	objects/
	refs/

该目录下有可能还有其他文件，但这是一个全新的 `git init` 生成的库，所以默认情况下这些就是你能看到的结构。新版本的 Git 不再使用 `branches` 目录，`description` 文件仅供 GitWeb 程序使用，所以不用关心这些内容。`config` 文件包含了项目特有的配置选项，`info` 目录保存了一份不希望在 `.gitignore` 文件中管理的忽略模式 (ignored patterns) 的全局可执行文件。`hooks` 目录保存了客户端或服务端钩子脚本。

另外还有四个重要的文件或目录：`HEAD` 及 `index` 文件，`objects` 及 `refs` 目录。这些是 Git 的核心部分。`objects` 目录存储所有数据内容，`refs`  目录存储指向数据 (分支) 的提交对象的指针，`HEAD` 文件指向当前分支，`index` 文件保存了暂存区域信息。



参考资料：

[Pro Git 简体中文版](https://iissnan.com/progit/)

