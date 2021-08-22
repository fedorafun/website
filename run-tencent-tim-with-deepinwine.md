---
title: 在Fedora下使用deepin-wine来水水群
description: 什么呀，熬到什么时候腾讯才会把他那个奥运纪念版的LinuxQQ给翻新一下啊？
slug: run-tencent-tim-with-deepinwine
timezone: UTC+8
date: 2021-08-22
image:
math:
license:
hidden: false
comments: true
---

> 在2019年10月24日，腾讯时隔多年放出了放出了LinuxQQ，不知多少Linux用户满怀欣喜地安装打开体验，然后又抱着失望而归。看来腾讯官方是指不上了，满足自己的日常工(shui)作(qun)需求还是得靠自己。
>
> 2020年的8月15日，一位名为 *v21cesc* 的网友在其[「FedoraApp分享空间」](http://v21cesc.ys168.com/)分享了其自行打包的`deepin-wine-all-in-one`，据我个人溯源应该是将`deepin-wine`打成rpm的第一人，为后续deepin-wine在Fedora上的使用奠定了基础（怎么感觉我在写历史书
>
> 本文我们将介绍一些Fedora下对于deepin-wine应用的使用方法和一些wine常见问题的基本处理思路。

## 安装deepin-wine（对于已经[添加fedorafun源](https://fedora.fun/p/fedora-fun-repo/)的用户可以跳过）

这一步很简单，我们只需要安装`deepin-wine-all-in-one`就可以同时安装deepin-wine和deepin-wine5（虽然现在基本已经是deepin-wine5的天下了）

我们可以从[「FedoraApp分享空间」](http://v21cesc.ys168.com/)下载以后使用`sudo dnf install + rpm包路径`的命令来安装

## 安装deepin-wine容器

deepin-wine容器的安装也很简单，你可以从[「FedoraApp分享空间」](http://v21cesc.ys168.com/)中找到由那位用户打包的一些deepin-wine容器，我们源里也自己打包、收录了一部分。下面的这张表里列举出了一些容器的包名，如果你添加了fedorafun源，你可以直接用`dnf`安装他们。

| 包名                | 描述                                            | 容器名       |
| ------------------- | ----------------------------------------------- | ------------ |
| com.qq.tim.spark    | 由星火商店打包的Tim，修复了头像图片不显示的问题 | Spark-TIM    |
| com.qq.weixin.spark | 由星火商店打包的微信                            | Spark-WeChat |
| com.qq.im.deepin    | 由deepin官方打包的QQ                            | Deepin-QQ    |

## 常见问题

### Deepin-wine应用的显示太小

deepin-wine不会主动地去适应高分屏，所以需要我们进行手动设置。

执行以下命令调出对于某一deepin-wine容器的设置面板:

```bash
WINEPREFIX=$HOME/.deepinwine/${容器名} deepin-wine5 winecfg
```

![wine的设置面板](https://pp1.edgepic.com/2021/08/23/498d90823030009.png)

这里的dpi取决于你想要缩放的大小，如果你是4k屏需要2倍缩放，可以调成192。当然，并不是只有高分屏能使用这个功能，即使你用的是1080P但还是觉得界面太小，你还是可以上调这个数值。

### Deepin-wine应用显示的是英文

这个可能是因为你的系统环境变量设置的是英文，我们可以直接在deepin-wine容器的启动脚本里修改环境变量来解决这个问题。

```bash
sudo sed -i "2i export LC_ALL=zh_CN.UTF-8" /opt/apps/${包名}/files/run.sh
```

### Deepin-wine应用中文字体异常

#### 语言环境变量问题

参考上一节

#### 字体问题

在`$HOME/.deepinwine/${容器名}/drive_c/windows/Fonts/`下粘贴或链接所需字体。

## 成果图

![最终成果图](https://pp1.edgepic.com/2021/08/23/637ca0823033647.png)
