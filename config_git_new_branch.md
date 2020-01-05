---
title: git如何创建一个分支
date: 2020-01-05
tag: [2020年, git]
category: 技术笔记
---

## git全局配置

git config --global user.name "xxx"
git config --global user.email "xxx@xxx.xxx"

查看git配置： git config -l

## git拉取远程分支并创建本地分支

git checkout -b 本地分支名称x origin/远程分支名称y

## 推送本地新建的分支到远程（remote)，以下命令会在远程端创建一个和本地分支名称完全相同的分支

git push origin 本地分支名称x  

## git add , git commit

命令中的.代表添加所有文件，也可以用实际的文件名称替代.： git add .
git commit -m "这里写上提交的内容的相关信息"

## git如何修改最后一次commit内容

git commit --amend

## git新建本地分支与远程分支关联，关联成功后，git push就会从push到关联的远程分支上

git branch --set-upstream-to=origin/远程分支名称x



