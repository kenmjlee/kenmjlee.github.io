---
title: Netgear R6250的固件果然还是DD-WRT最相配
date: 2018-01-20 21:43:00
categories:
   - 科技与技术
tags:
   - netgear
   - router
   - firmware
   - dd-wrt
   - tomato
description: 从DD-WRT安装到Tomato其实很简单，但笔者的经历可以用坎坷来形容。但作为一名中期懒癌伸手党，有些问题就懒得注册了去报，自己先记着。待有缘人看到再说吧。
---

koolshare团队的确是大牛，在advanced tomato的基础上加入了梅林的软件中心，令操作某些插件的确很方便。就这样入坑了。目前koolshare团队编译的[固件](http://koolshare.cn/thread-98787-1-1.html)基于3.4-138，是挺不错的版本。

从DD-WRT安装到Tomato其实很简单，但笔者的经历可以用坎坷来形容。但作为一名中期懒癌伸手党，有些问题就懒得注册了去报，自己先记着。待有缘人看到再说吧，

1.  下载固件 — 这小机率的事情也让给遇上了；下好了包准备开刷，等等，为什么包名是EA6700的？再下一次，下对了。R6250的！难道楼主发现放错了重新上传？于是仔细观察下，原来自xiaomi-R1DX以下的链接，型号和版本号中间的破折号“-”，它们的链接都是EA6700的链接，两边倒是对的……这也能碰上，注定接下来的周末就这样被废掉。

2.  刷机 — 问过谷歌大神，DD-WRT可以直刷，清nvram，刷后30-30-30就能用了。对吧，很简单吧？ 可是操作完变砖了。考虑到是不是DD-WRT的版本太旧，于是用nmrpflash救砖升到最新版，再来一遍……又砖了，无限重启。实在没办法了，想想可能还是从原厂刷入比较妥，于是重来一遍。从原厂到过渡固件一切正常，最后一步……双砖了。如是者从晚上8点到凌晨1点，来来回回把固件、nvram、硬重置弄了不下数十遍，仍然不成功。

    翌日，再试了好几遍还是不成功，可能睡好一觉思维清晰了不少。看看怎样逐一排查问题，第一件就是把外挂硬盘拔了。这一拔果真不得了，竟然启动成功提示输入帐号密码。不过，帐号竟然不是admin/admin，试了一千种可能包括网上说的。依然不行，又只能重刷了。一次成功，应该是ext[234]的驱动有问题，把硬盘换到NTFS就好了。

3.  DLNA — 能硬盘，当然是把BT和DLNA开上，变成视频中心。因为tomato里nvram保存的ms_dirs是"/mnt<"，所以这是无法在网页上修改它的路径，还一时手欠把这默认目录给删了后再也不能重新添加，唯一的办法就是重置。然后再也不敢乱改了。

4.  aria2 & aria2ui — 软件中心的aria2能设置的选项太少了，装了aria2ui后发现改过的设置每次重启或者过了一会儿就会变成默认值，不过对tomato没有太多的了解也不知道在哪里修改配置文件。也就放弃了，将就着用。

5.  USE支持 — 这也是要吐槽的一项，因为硬盘的挂载在不知不觉间就会sda->sdb->sdc这样切换，设置好的ftp路径，DLNA路径变会无效了。但因为ext驱动的原因，又不能对ntfs格式添加label，于是又无解了。

6.  WIFI — WIFI信号太弱应该是下狠心换回DD-WRT的诱因。无论改国家到新加坡、扫描一个没人用的频道、把功率加大，又或者把频宽从80 Mhz改到40 Mhz。曾经能连上WIFI的角落一直扫不出信号。运行一周后，就连能连的2.4G也变得无力，连上后也只能3Mbps, RSI 99 dBm。连最后的昏招，不隐藏SSID试了一点改善也没有。



写到这里并不是说advanced tomato有多不好，可能只是在R6250上表现不佳，毕竟这款路由也是奇葩（当初贪便宜买的）。但目前种种体验倒是令人怀念起DD-WRT（只要不是装了\$\$，dnsmasq会自杀），保证跑上个把月不是问题。就这样，又回归到DD-WRT的怀抱，希望迟一点能看到DD-WRT也有自己的软件中心，那这样切换\$\$节点和规则就轻松多了。