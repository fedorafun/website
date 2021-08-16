---
title: FedoraKDE用户的自我修养
description: 本文又名《作为一名Fedora的KDE用户，我们该有些什么B数》
slug: what-you-should-know-as-a-kde-user-of-fedora
timezone: UTC+8
date: 2021-08-05T19:57:25+08:00
image: 
math: 
license: 
hidden: false
comments: true
draft: false
---

Fedora的默认桌面是GNOME，所以作为一名KDE用户，咱心里就得有点B数了。

## 安装提要

安装的时候没什么要注意的，只需要在意下自己的FileSystem和分区方案就行了，我还是比较偏保守的，仍然在用ext4。还有就是要记得设置用户民和密码的时候要把下面的这个勾打上，不然创建的用户就不能使用sudo，到时候还要改`/etc/sudoers`，有点小麻烦的。如果确实忘记了，参考[archwiki](https://wiki.archlinux.org/title/Sudo_(%E7%AE%80%E4%BD%93%E4%B8%AD%E6%96%87)#%E8%AE%BE%E7%BD%AE%E7%A4%BA%E4%BE%8B)改一下。

![](img/2021-08-05_20-30.png)

## 登陆

在sddm登陆界面的时候，左下角可以选择使用wayland还是x11，作为KDE用户而言。。。我选择x11。需要提一句的是，如果你打算在虚拟机中使用Fedora，还是选择x11比较稳妥，vmware和vbox的增强工具目前在wayland下的兼容性并不是很OK。

![](img/2021-08-05_21-11.png)

## 换源更新

### 官方源

进入KDE界面了，咱决定换个源。虽然有用户表示Fedora的默认源在国内的速度不算太慢，但咱还是打算换个源，毕竟提升自己体验的同时也可以为国内减少国际出口流量，岂不美哉。

根据sjtug的文档，我们只需要一条简单的sed命令即可完成换源。

```bash
sed -e 's/metalink/#metalink/g' -e 's|#baseurl=http://download.example/pub/|baseurl=https://mirror.sjtu.edu.cn/|g' -i.bak /etc/yum.repos.d/<需要替换的文件>
```

那么我们来看看`/etc/yum.repos.d/`下的哪些文件是需要被替换的。

```
[zhullyb@fedora ~]$ ls /etc/yum.repos.d/
fedora-cisco-openh264.repo   fedora-updates.repo
fedora-modular.repo          fedora-updates-testing-modular.repo
fedora.repo                  fedora-updates-testing.repo
fedora-updates-modular.repo
```

目测了一下，除了第一个`fedora-cisco-openh264.repo`不能使用sjtug作为镜像源以外，其他源都可以更换。所以咱直接反手就是一条命令。

```bash
sudo sed -e 's/metalink/#metalink/g' -e 's|#baseurl=http://download.example/pub/|baseurl=https://mirror.sjtu.edu.cn/|g' -i.bak /etc/yum.repos.d/{fedora.repo,fedora-modular.repo,fedora-updates.repo,fedora-updates-modular.repo,fedora-updates-testing.repo,fedora-updates-testing-modular.repo}
```

### rpmfusion

Fedora官方源内的软件还是太匮乏，什么ffmpeg啦，obs-studio啦这种常见的软件都没有，赶紧整个rpmfusion.

#### 安装基础包

首先安装提供基础配置文件和 GPG 密钥的 `rpmfusion-*.rpm`，咱这里为了避免网络问题，是直接从bfsu镜像下载的基础包。

```bash
sudo yum install --nogpgcheck https://mirrors.bfsu.edu.cn/rpmfusion/free/fedora/rpmfusion-free-release-$(rpm -E %fedora).noarch.rpm https://mirrors.bfsu.edu.cn/rpmfusion/nonfree/fedora/rpmfusion-nonfree-release-$(rpm -E %fedora).noarch.rpm
```

#### 换源

咱这里用的是bfsu镜像。

```bash
sudo sed -e 's/metalink/#metalink/g' -e 's|#baseurl=http://download1.rpmfusion.org/|baseurl=https://mirrors.bfsu.edu.cn/rpmfusion/|g' -i.bak /etc/yum.repos.d/rpmfusion-*
```

### 完成

最后运行 `dnf makecache` 生成缓存。

现在即可更新系统啦，`sudo dnf upgrade`

## 输入法

然后我们还缺什么？当然是中文输入法啦，ibus那种老古董也就只有GNOME这种高度整合的桌面才考虑用一用啦，咱们KDE当然是用`fcitx5-chinese-addons`

```bash
sudo dnf install fcitx5-chinese-addons kcm-fcitx5 fcitx5-autostart
```

Fedora重启后，fcitx5将会被自动启动，接着你可以开始你自己的配置，我这里上一张成品图，具体的配置仍然可以参考[Archwiki](https://wiki.archlinux.org/title/Fcitx5_(%E7%AE%80%E4%BD%93%E4%B8%AD%E6%96%87))（当然，archwiki中提到的环境变量什么的在Fedora中统统不用加，仅参考词库导入、输入法配置、和自定义皮肤的章节即可）

![](img/2021-08-05_23-02.png)

## ~~卸载不常用的软件~~

~~虽然咱是国内用户嘛，但是Fedora还是和别的常见发行版一样非常**贴心**地给我们准备了非常多的我并不喜欢的软件，留着也是浪费硬盘空间，赶紧卸了。~~

## 终端字体

终端字体真的大力推荐`ttf-monaco`，来自Mac的字体，真的超护眼

```bash
wget https://storage.zhullyb.top/RPMs/ttf-monaco-6.1-6.noarch.rpm
sudo dnf install ./ttf-monaco-6.1-6.noarch.rpm
```

![](img/2021-08-05_23-10.png)

![](img/2021-08-05_23-12.png)

![](img/Screenshot_20210805_231256.png)

## 主题下载

这个主要还是要靠你自己去解决国内的网络问题，这边有个小工具咱还挺喜欢的。[v2rayA](https://github.com/v2rayA/v2rayA)
