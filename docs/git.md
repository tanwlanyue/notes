---
title: Git 基础
date: 2024-03-21 23:12:54
categories: 工具
tags:
  - git
---

[Pro Git 简体中文版](https://iissnan.com/progit/)

## SSH KEY

``` sh title="git配置" linenums="1"
# 生成 SSH Key
ssh-keygen -t rsa -C "tanwlanyue@gmail.com"
# 查看生成的 SSH 公钥
cat ~/.ssh/id_rsa.pub
# 测试
ssh -T git@gitee.com
```

## Config

``` sh
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
<!-- more -->

## Command

``` sh linenums="1" hl_lines="6 8"
# 删除分支
git branch -D branchName
# 修改最近的 commit message
git commit --amend
# 修改过去的 commit message
git rebase -i parentCommitId # (1)
# 合并连续的commit
git rebase -i parentCommitId # (2) 选择需要修改的commit  前面的pick改为s
# 查看最近三次提交记录
git log -3
# 分离头指针保存
git branch newBranchName commitId 
git checkout -b newBranchName branch/commitId
# 比较差异
git diff HEAD HEAD~1
```

1. 选择需要修改的commit  前面的pick改为r
2. 选择需要修改的commit  前面的pick改为s