---
title: 从 VS Code 配置 Github SSH 及踩过的坑
date: 2020-04-02 22:39:00
categories:
   - Tech
tags:
   - VS Code
   - github
   - macOS
   - ssh
description: 终于我又重操旧业，写起了代码。因为Visual Studio Code 每次 push 代码都要提示 username and password，所以决定改用 ssh key 解决问题。

---



还是个码农和比较 geek 的时候，一直是用 SourceTree 来 push 代码（包括这是 blog 的文章）。后来荒废了，最近又要重操旧业，决定一切都迁移到VS Code，连 SourceTree 都省了。但 Code 的最大问题是 Push 的时候总是要问用户名和密码，于是就转到 SSH 省点心。





### 生成 SSH Key

一、 在 macOS 下的 terminal 里使用 ssh-keygen 生在公钥和私钥文件，  

```bash
ssh-keygen -t rsa -b 4096 -C 'github的邮箱'
```

二、接着，命令会询问文件存放的位置和是否带密码， 直接无视就好了，  

```bash
Enter file in which to save the key (/Users/you/.ssh/id_rsa):
Enter passphrase (empty for no passphrase):    
Enter same passphrase again:    
```

三、看到以下就内容就生成好了，

```
+---[RSA 4096]----+
|.oo=             |
|o.=..   ..       |
|.o .o  o+.       |
|. .  =++...      |
|.    .B.S+ .     |
| .     Bo.o..    |
|..  . +..=o.o    |
|+. . . .= o*.    |
|ooo   .. ...Eo   |
+----[SHA256]-----+
```

四、将 id_rsa.pub 内容复制到 github 账号的 setting 下，   

```bash
`pbcopy < ~/.ssh/id_rsa.pub`
```




五、一切搞好就之后，就是紧张的刺激的踩坑环节！   



### 挖坑复挖坑，坑坑何其多

按常理，添加 ssh key 之后，使用 ssh-agent 就可以把私钥和公钥文件拿走，但添加 ssh-agent 时出现了以下内容：

> Permissions 0644 for 'id_rsa' are too open.
>
> It is required that your private key files are NOT accessible by others.
>
> This private key will be ignored.



What the hell! 

文件权限太高也是罪。只能将权限降到 600，`ssh -T git@github.com` 一切顺利。



#### 用 Visual Studio Code 拉取代码

现在，我们的 github 已经可以通过 ssh 进行各种操作，自以为在 VS Code 内也可以顺利之际，

> Git Error: Permission Denied

某种族人问号(??)，原来 VS Code 根本不支持 ssh-agent，怎办呢？

还好，Microsoft 这点上想得挺周到的，做了个叫 Remote Development 的 扩展，就是图中（我懒、省空间，图片是我“借”别人的）的这个，

![扩展](https://i.loli.net/2020/04/06/oqNJwMRWcpSnmd9.png 'Remote SSH')

装完之后，打开配置文件，把以下的都弄进去。完美！

```
Host github.com
  HostName github.com
  User github的username
  IdentityFile /Users/you/.ssh/id_rsa
```



![配置](https://i.loli.net/2020/04/03/agOFvzXVyH39QcN.png 'Configuration')





以上搞好以后，就可能愉快地使用 VS Code 管理代码了。是不是发现问题了？对的，就是如果不是想在 terminal 里面使用 git，那 ssh-agent 的意义根本就没有！！！
