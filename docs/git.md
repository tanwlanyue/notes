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
```

1. 选择需要修改的commit  前面的pick改为r
2. 选择需要修改的commit  前面的pick改为s
