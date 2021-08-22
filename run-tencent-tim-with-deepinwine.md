---
title: 在Fedora下使用deepin-wine来水水群
description: 什么呀，熬到什么时候腾讯才会把他那个奥运纪念版的LinuxQQ给翻新一下啊？
slug: run-tencent-tim-with-deepinwine
timezone: UTC+8
date: 2021-08-23
image:
math:
license:
hidden: true
comments: true
---

> 在2019年10月24日，腾讯时隔多年放出了放出了LinuxQQ，不知多少Linux用户满怀欣喜地安装打开体验，然后又抱着失望而归。看来腾讯官方是指不上了，满足自己的日常工(shui)作(qun)需求还是得靠自己。
>
> 2020年的8月15日，一位名为 *v21cesc* 的网友在其[「FedoraApp分享空间」](http://v21cesc.ys168.com/)分享了其自行打包的`deepin-wine-all-in-one`，据我个人溯源应该是将`deepin-wine`打成rpm的第一人，为后续deepin-wine在Fedora上的使用奠定了基础（怎么感觉我在写历史书
>
> 本文我们将介绍一些Fedora下对于deepin-wine应用的使用方法和一些wine的基本操作。

## 安装deepin-wine（对于已经[添加fedorafun源](https://fedora.fun/p/fedora-fun-repo/)的用户可以跳过）

这一步很简单，我们只需要安装`deepin-wine-all-in-one`就可以同时安装deepin-wine和deepin-wine5（虽然现在基本已经是deepin-wine5的天下了）

我们可以从[「FedoraApp分享空间」](http://v21cesc.ys168.com/)下载以后使用`sudo dnf install + rpm包路径`的命令来安装

## 安装deepin-wine容器

deepin-wine容器的安装也很简单，你可以从[「FedoraApp分享空间」](http://v21cesc.ys168.com/)中找到由那位用户打包的一些deepin-wine容器，我们源里也自己打包、收录了一部分。下面的这张表里列举出了一些容器的包名，如果你添加了fedorafun源，你可以直接用`dnf`安装他们。

| 包名                | 描述                                            |
| ------------------- | ----------------------------------------------- |
| com.qq.tim.spark    | 由星火商店打包的Tim，修复了头像图片不显示的问题 |
| com.qq.weixin.spark | 由星火商店打包的微信                            |
| com.qq.im.deepin    | 由deepin官方打包的QQ                            |

