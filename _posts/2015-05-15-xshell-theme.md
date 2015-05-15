---
layout : post
title : "Xshell配色设置"
category : "工具"
tags : [xshell,theme]
---

Xshell 是一个强大的安全终端模拟软件，最近我在使用xshell最为连接linux服务器的工具。

个人觉得它要比SecureCRT好用的一点。

但是当连接到Linux服务器后，它自带的配色真的是亮瞎了我的双眼，文件夹的蓝色....我根本看不到。

无奈之下，个人在网上找了三个xshell的配色方案。

## 三种配色方案

1, 第一种

```
[ubuntu]
text(bold)=ffffff
magenta(bold)=ad7fa8
text=ffffff
white(bold)=eeeeec
green=4e9a06
red(bold)=ef2929
green(bold)=8ae234
black(bold)=555753
red=cc0000
blue=3465a4
black=000000
blue(bold)=729fcf
yellow(bold)=fce94f
cyan(bold)=34e2e2
yellow=c4a000
magenta=75507b
background=300a24
white=d3d7cf
cyan=06989a
[Names]
count=1
name0=ubuntu
```

2, 第二种

```
[isayme]
text(bold)=eaeaea
magenta(bold)=ff00ff
text=ffffff
white(bold)=eaeaea
green=00c000
red(bold)=d20000
green(bold)=00ff00
black(bold)=808080
red=c00000
blue=113fcc
black=000000
blue(bold)=0080ff
yellow(bold)=ffff00
cyan(bold)=00ffff
yellow=c0c000
magenta=c000c0
background=222222
white=c0c0c0
cyan=00c0c0
[Names]
count=1
name0=isayme
```

3, 第三种

```
[Solarized Dark]
text(bold)=839496
magenta(bold)=6c71c4
text=839496
white(bold)=fdf6e3
green=859900
red(bold)=cb4b16
green(bold)=586e75
black(bold)=073642
red=dc322f
blue=268bd2
black=002b36
blue(bold)=839496
yellow(bold)=657b83
cyan(bold)=93a1a1
yellow=b58900
magenta=dd3682
background=042028
white=eee8d5
cyan=2aa198
[Names]
count=1
name0=Solarized Dark
```

将这三个分别保存为 __ubuntu.xcs__,__isayme.xcs__,__Solarized\_Dark.xcs__。

## 使用

打开xshell

![](http://isharec.qiniudn.com/QQ图片20150515140210.png)

点击配色

将刚才保存的三种配色方案导入即可。

目前我使用的是Solarized Dark这种配色方案。