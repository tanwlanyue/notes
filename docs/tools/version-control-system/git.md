---
title: Git 基础
date: 2024-03-21 23:12:54
categories: 工具
tags:
  - git
---
[Pro Git 简体中文版](https://iissnan.com/progit/)

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





