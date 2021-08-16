---
title: FedoraFun仓库指北
description: 好诶，咱们有自己的源了
slug: fedora-fun-repo
timezone: UTC+8
date: 2021-08-12T03:53:00+08:00
image: 
math: 
license: 
hidden: false
comments: true
draft: false
---

> 拖了得有大概小半个月了，总算是把我们FedoraFun的源支起来了。

## 添加命令

```bash
sudo sh -c "echo '[fedorafun]
name=fedorafun
baseurl=https://repo.fedora.fun/
enabled=1
countme=1
metadata_expire=7d
repo_gpgcheck=0
type=rpm
gpgcheck=0
skip_if_unavailable=False' > /etc/yum.repos.d/fedorafun.repo"

sudo dnf makecache
```

## Package List

添加了源以后，就是一些`dnf`的常规操作啦，应该难不倒你们，下面是一份**Package List**，请查收

| 包名                    | 版本号              | 来源                                                         | 注                                                           |
| ----------------------- | ------------------- | ------------------------------------------------------------ | ------------------------------------------------------------ |
| com.alibabainc.dingtalk | 1.0.0.203-1         | 基于[AUR](https://aur.archlinux.org/packages/com.alibabainc.dingtalk/)二次打包 | 钉钉内测                                                     |
| com.antutu.benchmark    | 1.0.0.590-1         | 基于[AUR](https://aur.archlinux.org/packages/com.antutu.benchmark/)二次打包 | 安兔兔评测                                                   |
| com.qq.tim.spark        | 3.3.5.22018spark0-1 | 基于[AUR](https://aur.archlinux.org/packages/com.qq.tim.spark/)二次打包 | wine-tim                                                     |
| com.qq.weixin.spark     | 3.2.1.127spark0-1   | 基于[AUR](https://aur.archlinux.org/packages/com.qq.weixin.spark/)二次打包 | wine-wechat                                                  |
| deepin-wine-all-in-one  | 1.0-1               | 搬运自[「FedoraApp分享空间」](http://v21cesc.ys168.com/)     | deepin-wine                                                  |
| fcitx5-pinyin-zhwiki    | 0.2.3.20210804-1    | 基于[Archlinux](https://archlinux.org/packages/community/any/fcitx5-pinyin-zhwiki/)二次打包 | 肥猫维基百万大词库                                           |
| freechat-git            | 1.0.0.3a8304e-1     | 基于[源码](https://github.com/eNkru/freechat/)编译打包       | 基于网页版微信，添加UOS补丁                                  |
| icalingua               | 2.1.5-1             | 搬运自上游[Release](https://github.com/Clansty/Icalingua/releases/) | electron-qq                                                  |
| Motrix                  | 1.6.11-1            | 搬运自上游[Release](https://github.com/agalwood/Motrix/releases/) | Motrix                                                       |
| net.winegame.client     | 0.5.7.2-3           | 基于官网包重打                                               | 修复在Fedora 34上表现出的错误依赖                            |
| netease-cloud-music     | 1.2.1-3             | 基于[AUR](https://aur.archlinux.org/packages/netease-cloud-music-imflacfix/)二次打包 | 网易云音乐，添加qcef与vlc补丁以修复输入法和高品质音乐播放问题 |
| purewriter-writer-bin   | 1.3.4-2             | 基于[AUR](https://aur.archlinux.org/packages/purewriter-desktop-bin/)二次打包 | 纯纯写作Desktop                                              |
| qqmusic-bin             | 1.1.0-3             | 基于[AUR](https://aur.archlinux.org/packages/qqmusic-bin/)二次打包 | QQ音乐                                                       |
| qtscrcpy                | 1.6.0-1             | 基于[AUR](https://aur.archlinux.org/packages/qtscrcpy/)二次打包 |                                                              |
| ttf-monaco              | 6.1-6               | 基于[AUR](https://aur.archlinux.org/packages/ttf-monaco/)二次打包 | MacOS终端御用字体                                            |
| wechat-uos              | 2.0.0-2             | 基于[AUR](https://aur.archlinux.org/packages/wechat-uos/)二次打包 | 基于网页版微信，添加UOS补丁                                  |
