---
layout : post
title : "Ubuntu环境下，Spyder中Matplotlib中文字体显示问题"
category : "Linux"
tags : [Anaconda,Spyder,Matplotlib]
---

## 前言

在使用Ubuntu16.04操作系统时，在Ubuntu上安装了Anaconda3，在使用Spyder进行数据分析师，发现Matplotlib中文字体都显示为一个个的方框。

## 解决步骤

首先我们需要去下载SimHei字体。

1、安装字体

拷贝字体到`usr/share/fonts`下

2、删除缓存

执行如下代码，获取缓存路径。

```py
import matplotlib as mpl 
mpl.get_cachedir()
```

进入对应文件夹，删除里面的全部内容。

3、修改配置文件

执行如下代码，获取配置文件路径。

```py
import matplotlib as mpl 
mpl.matplotlib_fname()
```

编辑`matplotlibrc`文件，将以下内容写入文件末尾。

```
font.family         : sans-serif
font.sans-serif     : SimHei
axes.unicode_minus  : False
```

4、重构字体

执行如下代码：

```
from matplotlib.font_manager import _rebuild
_rebuild()
```

再次使用Matplotlib作图就可以发现中文显示正常了。