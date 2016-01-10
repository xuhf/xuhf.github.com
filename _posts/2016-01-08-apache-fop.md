---
layout : post
title : "Apache FOP简介"
category : "翻译"
tags : [PDF]
---

## 介绍

![](http://7s1rw0.com1.z0.glb.clouddn.com/site.jpg)

`Apache FOP`（格式化对象处理器）是一个使用XSL格式化对象（`XSL-FO`）和输出独立的打印格式化驱动。

它是一个读取格式化（FO）树，并呈现结果页面到一个特殊的输出的Java应用程序。

目前支持的输出格式包括PDF，PS，PCL，AFP，XML（区域树表现），Print，AWT和PNG，并在较小的范围内，支持RTF和TXT。主要的输出目标是PDF。

Apache FOP项目是Apache软件基金会的一部分，Apache软件基金会的开源项目拥有广泛的社会用户和开发者。

![](http://7s1rw0.com1.z0.glb.clouddn.com/document.jpg)

FOP最新的可获得版本是FOP 2.0。

在`FOP Compliance`中，每一个对象和属性的规范支持是详尽的。下载选项包括一个预编译的版本，源代码，和一些例子文件来让你开始学习FOP。

资源包括到XSL-FO的介绍链接和一些其他有用的参考。开始帮助的检查列表能够引导你最大限度的使用FOP。

FOP为Apache的XML图形项目的一部分而自豪。

## 示范

![](http://7s1rw0.com1.z0.glb.clouddn.com/layout.jpg)

此图片是两页真实文档的展示。左侧的XML数据被格式化成右侧的两页。这个文档包含出现在每一页，外部图形，第一页的脚注，以及跨两页的表格的静态区域。

FOP使用标准的XSL-FO文件格式作为输入，当请求输出的时候渲染排列内容输出到页上。

使用XSL-FO作为输入一个很大的优点是XSL-FO本身就是一个XML文件，这意味着它可以方便的从各种来源创建。

最常见的方式是使用XSLT转换语义XML到XSL-FO。

## FOP目标

Apache FOP项目的目标是传递一个XSL-FOD到PDF的格式，至少兼容从2006年12月5号起W3C建议的基本一致性级别，并且实现2001年11月从Adobe系统可移植文档格式规范（版本1.4）。

兼容XML1.0和1.1建议，XSLT1.0和2.0建议，和XML命名空间建议的理解。其他有关文件，例如XPath和XLink Working Drafts（XLink工作草案），作为参考是必要的。

FOP项目将尝试使用不断发展的规范的最新版本。

伯乐在线地址：<http://hao.jobbole.com/?p=9957>

官方网站：<http://xmlgraphics.apache.org/fop/>

开源地址：<http://svn.apache.org/repos/asf/xmlgraphics/fop/>
