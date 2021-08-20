---
hidden: true
---

### 是存放fedorafun博客markdown文件的Github仓库！

fedorafun采用hugo框架构建其博客，本仓库对应`content/post`路径。每一次push后，Github Action（随手手糊的，希望不会出问题）将会调用vercel的api，使其自动完成构建，f因此本仓库的每一次变动都会直接影响到[fedora.fun](https://fedora.fun)。

我们当然欢迎你来参与fedorafun的博客编写，这也是我们把这一路径单独独立出来扔Github公开库的初衷。

markdown文件的开头应该长成这个样子：

```
---
title: "Example"
description: 
slug: 
timezone: UTC+8
date: 2021-08-20
image: 
math: 
license: 
hidden: false
comments: true
---
```

- title填写文章标题，

- description用于填写描述信息，或者你也可以理解为副标题

- slug决定了文章的链接，直接填写markdown的文件名（不带文件类型后缀），要求使用英文

- date填写日期即可

其他的就没什么讲究了，接下来使用markdown语法继续书写内容即可。对于文中需要使用的图片可以直接使用第三方图床，但**要求自行备份原图片**。如果你想要署名，我们不会反对在文章结尾处以书信格式留下你的昵称、邮箱以及你的技术博客，当然了，**推广is not acceptable**。

完成一份markdown文件的书写以后，需要你fork本仓库并提个pr，我们收到后将会进行**~~及时~~**的审核。
