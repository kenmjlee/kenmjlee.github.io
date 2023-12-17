---
title: 关注写作就好，把Hexo部署交给Travis CI
date: 2017-12-12 00:18:26
tags:
   - TravisCI
   - Hexo
   - Github
   - Automation
categories: 
   - 科技与技术
description: 不久之前还在使用Jekylly部署github page，每次都要在本地编译后再push到repo上。于是写了几篇博客又搁置了。后来在研究Travis CI时，Google出很多人都在用它来部署github page，于是再次入坑。
---



曾经断断续续地在各类博客网站写过日记，可是倒闭的倒闭，被封的被封，要么就是主题太丑操作不便。直至遇上了github page + Jekyll，不过还要自己改template又要每次都要本地编译部署，写着写着又放弃了。后来在研究Travis CI，偶然发现不少大牛都在使用它来部署github page，于是再次入坑。

Travis CI跟Jenkins一样，都是持续集成的工具。但对于个人开发者来说，毕竟Jenkins太重了，要不是为了研究Jenkins，才不会为自己的开源项目搭一个Jenkins，对吧？Travis CI的Free for Open Source的确帮了不少忙。

在Travis关联好github帐号、Repo和Branch。在项目根目录下加上_**.travis.yml**_文件，当有push时，Travis就会按配置文件进行一系列的操作。

```
language: node_js

node_js: stable

cache:
    apt: true
    directories:
        \- node_modules
before_install:
  \- npm install -g hexo
  \- npm install -g hexo-cli
install:
  \- npm install
script:
    \- hexo cl
    \- hexo g

after_script:
  \- cd ./public
  \- git init
  \- git config user.name "kenmjlee"
  \- git config user.email "kenneth.mj.lee@gmail.com"
  \- git add .
  \- git commit -m "Travis CI Auto Builder at `date +"%Y-%m-%d %H:%M"`"
  \- git push --force --quiet "https://${GH_TOKEN}@${GH_REF}" master:master

branches:
    only:
        \- source
env:
    global:
        \- GH_REF: github.com/kenmjlee/kenmjlee.github.io.git
```



网上不少朋友说因为GH_Token会明文保存，需要通过加密。其实Travis已经支持在配置页上添加Environment Variables。![Setting Page](/uploads/travis-ci-setting-environment-variables.png)

配置好以后，安心写作。剩下的关由Travis去管吧。