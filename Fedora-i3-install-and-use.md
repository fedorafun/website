---
title: "《Fedora i3 生存指南》"
description: 关于在全都是Gnome用户下的情况下的i3wm生存指南 
slug: Fedora-i3-install-and-use
timezone: UTC+8
date: 2022-01-25
image: 
math: 
license: 
hidden: false
comments: true
---


>第一次探索wm，只能说i3太神奇了
# Fedora i3 生存指南

> 1.安装!

> 2.换源
>
> >2.1 官方源
> >
> >2.2 第三方源
> >
> >> 2.2.1 rpmfusion
> >>
> >> 2.2.2 fedorafun

> 3.软件安装
>
> > 3.1  terminal 安装
> >
> > 3.2  渲染器安装
> >
> > 3.3  输入法安装
> >
> > 3.4(可选)rofi安装
> >
> > 3.5(可选) i3-gaps/sway安装

> 4. 基础设置

> > 4.1 高分屏设置
> >
> > 4.2  触控板设置
> >
> > 4.3亮度快捷键
> >
> > 4.4 蓝牙设置

> 5. 如何配置应用自启动及其Fedora i3 默认快捷键及其如何更设置改或如何新增加快捷键

> > 5.1 如何配置应用开机自启动
> >
> > 5.2 Fedora i3 常用默认快捷键以及如何更改快捷键

> > > 5.2.1  Fedora i3 常用默认快捷键
> > >
> > > 5.2.2如何更改或者新增快捷键

> 6 个性化设置(可选择)

> > 6.1 如何做到自动切换壁纸
> >
> > 6.2 怎么设置窗口间隙 
> >
> > 6.3 怎么实现窗口透明呢？
> >
> >  6.4如何干掉难看的标题栏呢？
> >
> > 6.5 如何设置主题呢?
> >
> > 

## 0.1 前言：

由于Fedora的官方维护桌面为gnome所以i3使用起来可能不会那么的顺手，不过还是可以用的！

##  1. 安装

插入安装盘进入Fedora i3 时，会于其他桌面有一点不同，会弹出两个两个对话框，凭借自己喜好选择便是了，然后使用你所使用的$mod+Enter键呼出终端输入:   liveinst 便可以开始你的Fedora安装之路，这里没什么好说的，唯一有一点需要注意的是在创建用户时候，请记得将 “此用户设为管理员选上” 否则你安装完成之后发现无法使用sudo命令，不过没事，你只修改 /etc/sudoers 将你的用户添加便可以了，具体可以参考[archwiki](https://wiki.archlinux.org/title/Sudo_(简体中文)#设置示例)，其他选项凭借个人喜好便可

## 2.换源

来到国外的发行版第一件事就是给它换源啦！换上之后可以大大加速下载速度那么如何换源呢？

### 2.1. 官源 我们使用 sjtug或者bfsu，大差不差，这里我使用 sjtug作演示，在终端输入:

``` bash
sudo sed -e 's/metalink/#metalink/g' -e 's|#baseurl=http://download.example/pub/|baseurl=https://mirror.sjtu.edu.cn/|g' -i.bak /etc/yum.repos.d/{fedora.repo,fedora-modular.repo,fedora-updates.repo,fedora-updates-modular.repo,fedora-updates-testing.repo,fedora-updates-testing-modular.repo}
```

  就ok啦！

------



### 2.2 第三方源 

官方源里的软件还是太少太少了，连obs-studio,ffmpeg这类常用软件都没有，所以我们使用第三方源来解决这些问题

#### 2.2.1 rpmfusion

第一步下载基础包(开源和闭源)， 这里我们使用bfsu来下载，以避免网络问题，终端输入:

``` bash
sudo yum install --nogpgcheck https://mirrors.bfsu.edu.cn/rpmfusion/free/fedora/rpmfusion-free-release-$(rpm -E %fedora).noarch.rpm https://mirrors.bfsu.edu.cn/rpmfusion/nonfree/fedora/rpmfusion-nonfree-release-$(rpm -E %fedora).noarch.rpm
```

第二步，使用bfsu镜像

``` bash
sudo sed -e 's/metalink/#metalink/g' -e 's|#baseurl=http://download1.rpmfusion.org/|baseurl=https://mirrors.bfsu.edu.cn/rpmfusion/|g' -i.bak /etc/yum.repos.d/rpmfusion-*
```

---

#### 2.2.2 Fedorafun

这里有一些国内常用软件，以及一些开源软件,终端输入：

``` bash
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
```

然后我们就可以开始 快乐的 update啦！

``` bash
sudo dnf makecache && sudo dnf update -y
```

------

### 3.  软件安装

新出生的i3就如未装修的毛胚房一般，需要我们自己安装一系列的软件来对它进行装饰和改造，那么，来吧！

---

#### 3.1 终端的安装

默认的 i3-sensible-terminal 简直不想是能用的，所以我们需要更换终端来替换它，一般来说我们使用的有 xfce4-terminal , alacritty，tilix，这里我推荐alacritty，因为它有GPU加速，不过最后可以凭借你的喜好来选择！

我们以来命令来安装使用alacritty:

``` bash
dnf in -y  alacritty  
```

然后我们需要修改一下默认终端快捷键，来方便我们启动，我们需要编辑i3的config文件(这里是i3的配置文件，快捷键什么的都在这里)，这里我使用nvim，也可凭借个人喜好

``` bash
nvim ~/.config/i3/config
```

然后找到terminal字样将$mod+Return exec 后的   i3-sensible-terminal 替换为你安装的终端，这里我使用 alacritty 就换成alacritty，具体图如下:



 ![image-20220124201811809](/home/lin/.config/Typora/typora-user-images/image-20220124201811809.png)

ok啦然后保存推出你所使用的编辑器，使用Shift+$mod+C来立刻刷新你的i3配置,alacritty还有许多可以配置的地方，这里就不一一列举了，感兴趣可以看[Archwiki](https://wiki.archlinux.org/title/Alacritty)

---

#### 3.2 渲染器安装

渲染器是Linux桌面中重要的组件，可i3不自带渲染器所以我们需要安装一个，这里我们选用compton作为我们的渲染器,终端输入:

``` bash
sudo dnf in picom -y
```

就完成了我们渲染器的安装，接下来，我们需要在i3的配置文件中添加配置以让渲染器开机自动启动,在i3配置文件中添加:

``` bash
 exec --no-startup-id picom -b
```

然后我们就完成了我们渲染器的安装和自启动

---

#### 3.3 输入法安装

中文输入法作为我们常用的软件怎么能少的了它呢？这里我选择fcitx5来安装，终端输入:

``` bash
sudo dnf in fcitx5-chinese-addons kcm-fcitx5 fcitx5-autostart -y 
```

添加自启动:

``` bash
exec_always --no-startup-id fcitx5
```

然后我们可以使用Ctrl+Shift+Alt+C来启用云拼音,若想要添加主题可看[Archwiki](https://wiki.archlinux.org/title/Fcitx5_(%E7%AE%80%E4%BD%93%E4%B8%AD%E6%96%87)#%E4%B8%BB%E9%A2%98%E5%92%8C%E5%A4%96%E8%A7%82)

---

#### (可选)3.4 rofi 安装

rofi是一个程序启动器，安装它可以使用

``` bash
sudo dnf in rofi
```

若使用rofi可以替换原来的dmenu，通过修改i3配置文件，将$mod+d exec --no-seartup 后面的内容换成rofi -show drun即可，图示:

![image-20220124215243345](/home/lin/.config/Typora/typora-user-images/image-20220124215243345.png)

---

### (可选)3.5 i3-gaps  sway

i3-gaps是i3的分支，比起i3有着窗口缝隙等特点，可以使你的i3更美观，那么我们要如何安装呢？很简单，我们只需要输入:

``` bash
sudo dnf remove i3 && sudo dnf in i3-gaps -y 因为 i3-gaps是i3的分支，所以我们需要删除i3才可以安装i3-gaps
```

---

sway 是 基于waydland开发的配置文件完全兼容i3的wm（可以理解为i3的wayland版本）这个安装也同样简单

``` bash
sudo dnf in sway -y 
```

然后dm启动时选sway即可

说明:这一栏仅仅是我本人推荐，请结合个人喜好和需求安装。

---

### 4 基础设置

---

#### 4.1 高分屏

在~/.Xresources中修改xft.dpi:xxx ，dpi数值因屏而异，请自行调整

---

#### 4.2 触控板

触控板手势在默认的i3中触摸板非常难用，没有触摸点击，自然滚动等，不过我们可以通过修改/usr/share/X11/xorg.conf.d/40-libinput.conf,这一配置文件来配置,在找到 libinput touchpad catchall 的关键词下配置中的Driver中另起一行填入:

``` bash
Option "Tapping" "on"    # 开启模拟单击
Option "NaturalScrolling" "on"    # 开启自然滚动
Option "TappingButtonMap" "lrm"    # 开启双指点击=鼠标右键,三指点击=鼠标中键
Option "AccelProfile" "adaptive"  # 鼠标加速
Option "AccelSpeed" "0.4"   # 鼠标指针的加速速度
```

---

#### 4.3 亮度快捷键

Fedora自动配置了音量快捷键，但不知道为什么没有亮度快捷键，对于这个我们可以通过安装xfce的xfce4-power-manager来解决这个问题，也很简单，我们只需要安装xfce4-power-manager:

``` bash
sudo dnf in xfce4-power-manager
```

然后在i3的配置文件中加入:

``` bash
exec --no-startup-id xfce4-power-manager
#exec --no-startup-id spectacle -s
exec --no-startup-id /usr/lib/xfce4/notifyd/xfce4-notifyd
exec --no-startup-id xss-lock -- i3lock -n -c 000011
```

然后加载一下配置文件就可以用了

---

#### 4.4 蓝牙

i3默认是没有蓝牙的，若需要蓝牙则需要安装blueman，终端输入:

``` bash
sudo dnf in blueman -y
```

然后

``` bash
sudo systemctl --now enable blueman 
```

然后重启电脑运行blue manager就可以啦！

---

### 5.如何配置应用开机自启动及其Fedora i3 默认快捷键

#### 5.1 如何配置应用开机自启动

很多时候我们有服务需要在开机的时候运行，如v2raya等，这是如何启动呢？除了systemd服务以外我们可以使用i3的配置文件来实现应用开机自启动而且很简单不需要写复杂的systemd服务，我们可以在i3配置文件中加入:

``` bash
exec --no-startup-id  应用
```

就ok了！

---

#### 5. 2.1 Fedora i3 常用默认快捷键

这里我会列出一些常用的默认快捷键，可以自行查阅后去config文件中修改

| $mod+Enter           | 打开终端                 | $mod+shift+e     | 退出i3wm              |
| -------------------- | ------------------------ | ---------------- | --------------------- |
| $mod+Enter+V         | 向下打开一个终端         | $mod+1~9~0       | 切换工作区            |
| $mod+<-- or $mod+--> | 左右切换窗口             | $mod+Shift+e     | 退出i3wm              |
| $mod+Shift+Q         | 退出窗口                 | $mod+Shift+C     | 立刻刷新I3配置        |
| $mod+F               | 当前窗口全屏             | $mod+W or $mod+E | 将当前窗口向左移/右移 |
| $mod+d               | 打开fedora i3自带的dmenu | $mod+ S          | 堆叠显示窗口          |
| $mod + W             | 标签显示窗口             | $mod+e           | 默认平铺显示窗口      |
| $mod + Shift + R     | 重启i3wm并刷新布局       | 有待补充.....    | 有待补充.....         |
|                      |                          |                  |                       |
|                      |                          |                  |                       |

---

#### 5.2.2如何更改快捷键

也许你会觉得你的快捷键不够顺手或者你想新增快捷键，那我们又要如何实现呢，也是通过修改i3配置文件来实现，要修改，我们就要知道我们自己键盘的键位是什么，一共有mod1,mod2,mod3,由于每个键盘上的mod不同，所以请你一个个试吧！如何更改或者新增呢，打开我们的i3 config文件，不难发现，每个快捷键的语法都是:

``` bash
bindsym $mod+xxx应用程序 参数
```

要修改或者新增，照葫芦画瓢便是了。

---

### 6 个性化设置(可选)

---

#### 6.1如何自动切换壁纸呢？

一直使用同一个壁纸也是蛮枯燥的，我们可以通过安装 feh 来实现随机换壁纸，先安装feh:

``` bash
sudo dnf in feh -y
```

然后在i3的配置文件中加入:

``` bash
exec --no-startup-id feh --bg-fill $(cat 壁纸存放目录)
```

 目录中一定要有壁纸哦！

---

#### 6.2 如何设置创建间隙

看到好看的i3配置，窗口间隙少不了！那么我们如何使用窗口间隙呢？很简单！只要在安装完i3-gaps之后就在i3的config文件中新增:

``` bash
gaps inner xxx # xxx为间隙大小值
```
---

#### <a id="appendix">6.3</a>如何设置终端半透明呢？ 

看到漂亮的i3配置中他们的终端很大一部分都是透明呢，这又是如何设置的呢？也很简单，安装完渲染器并且渲染器运行后我们只需要在你的终端设置中来设置透明。如xfce4-terminal，图示:

![image-20220124232904438](/home/lin/.config/Typora/typora-user-images/image-20220124232904438.png)

---

#### 6.4 怎么干掉难看的标题栏呢？

在<a href="#appendix">6.3</a>中看到了这个难看到标题顶栏，那么我们要如才能干掉他呢？同样也很简单，我们只需要简单的一段配置即可，使用你的编辑器，在i3 config文件中添加:

``` bash
new_window 1pixel
```

然后刷新i3 配置就可以啦！

---

#### 6.5 如何设置主题呢？

觉得一个颜色太单调了吗？i3下也是有主题的！只需要安装一个lxappearance,终端输入:

``` bas	
sudo dnf in lxappearance -y
```

然后去查找主题和图标吧！

