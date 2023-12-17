---
title: 跨平台同步VS Code 插件和配置
date: 2020-05-01 11:00:00
categories:
   - 科技与技术
tags:
   - VS Code
   - github
description: 不需要账号登录的VS Code是怎样实现将插件和配置同步，减少机器重装后的烦恼。除了使用云盘之类的服务外，还有什么方法保证各个环境一致？

---

没有云备份的应用，一般都是使用网盘之类的服务，但单单只是为了一两个配置就有点麻烦，而且配置文件多数是和应用的其它文件放在一起，那就要先导出配置文件，同步后再导入。作为懒惰的老码农，当然希望是有更便捷的方法。拜访古狗大神之后，找到setting sync 这个插件。使用 gist 作为保存配置的它，的确比其它办法方便。

## 两个方法的关联 github

#### 1. 先在 github 创建一个 public 或 secret gist

public gist 可以让你的团队共同分享一个配置，而secret gist 当然就是只有你自己可以访问的配置了。

#### 2. 安装 Setting Sync 插件

![安装](https://i.loli.net/2020/05/01/jL72WJ1cIRFNdyO.png 'Install Setting Sync')

#### 3. 关联 github

![登录 github](https://i.loli.net/2020/05/01/vOIakKGN6oY4B2e.png 'Login github')
这是一种选择，好处是不用去 Developer Setting 页面创建 token，因为 token 一旦创建就要保存下来，以后就不能再看到了。

当登录后，页面会列出 gist，选择一个作为保存配置的即可。
再使用
>sync: update/upload extensions
>sync: download extensions

就可以在不同的平台上同步了。