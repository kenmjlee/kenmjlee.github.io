---
title: 天猫魔盒亮灯黑屏TTL救砖记
date: 2018-02-06 17:04:00
categories:
      - 技术风
tags:
     - 天猫魔盒
     - TMB100A
     - TTL
     - kodi
description: 小米电视3装了Kodi 1.7后，想着也在一直掉角落没用过的TMB100A上装装看。
---

最近在小米电视3装了Kodi 1.7后，想着也在一直掉角落没用过的TMB100A上装装看。可惜天猫魔盒1的ROMs都是基于Android 4.2，只能装Kodi 1.6但python库又对新addons不友善。后来看到有大神在盒上安装Lakka，或者LibreELEC，所以也试着安装一下。



**（利申：笔者非硬件派，对于AML CPU整个系统的启动机制、命令并不熟悉。纯属按网文及自己研究）**



一开始是用wetek的Lakka ROM去安装，估计缺了文件。短接后一直黑屏没亮灯。之后刷的是[LibreELEC](https://forum.libreelec.tv/thread/4858-8-0-2-libreelec-8-0-builds-for-mx2-g18/)，按post介绍用ext3，短接没反应；改回了SDMaker重制了一次，变成不断重进REC刷ROM。粗暴断电后再开，就变成亮灯黑屏了。用了原厂firm和CTK-REC，能进。但刷完还是黑屏。遍历Google，这情况应该是NAND有坏区。无奈之下还是在淘宝买了一条PL2303。要留意的是，国产的PL2303在Windows 10下必须是用2009的旧版driver。原因大家懂。 

通过putty连接后终于看到网上各位大神说的界面，输入`nand bad`和`nand scrub_safe`应该就可以了。 可怜笔者的盒子还是一样。就在这时，putty的界面再没有输出，重连几次也是一样，正当怀疑人生之际，原来TTL线松了……花了点时间重新接上，再次执行了命令。哈，终于成功了。见到熟悉的病猫钓鱼画面。



###### 后记

其实TTL本来早该买了，此前试过把netgear变砖很多教程都说要TTL来救。后来用nprmflash能远程救砖就没买了。这次也是想着试一下远程IP刷盒，一直在尝试浪费不少时间。



