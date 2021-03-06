---
layout : post
title : "git查看某个文件的修改历史"
category : "Git"
tags : [git]
---

## 开始

有的时候，我们需要查看文件的某些改动，某一个修改都修改了哪些文件。

以下的git命令可以帮助到我们

## git blame filename

显示文件的每一行是在哪个版本最后修改

```sh
$ git blame README.md

e130233b (vvkee 2015-04-21 19:20:13 +0800 1) ## 项目简介
^28a840a (vvkee 2015-04-14 22:44:47 +0800 2)
e130233b (vvkee 2015-04-21 19:20:13 +0800 3) vvkee的个人博客
^28a840a (vvkee 2015-04-14 22:44:47 +0800 4)
6c40c0df (vvkee 2015-05-14 14:08:46 +0800 5) <http://www.vvkee.com>
```

## git whatchanged filename

显示文件的每个版本提交信息。

```sh
$ git whatchanged README.md

commit 6c40c0df95b929da59da645ffde0a33f54ff6a3f
Author: xx <xx@xx.com>
Date:   Thu May 14 14:08:46 2015 +0800

    modify domain

:100644 100644 b82f668... e4f7342... M  README.md

commit e130233b2eb3d786b7bc36eb90f9560ae751fda9
Author: xx <xx@xx.com>
Date:   Tue Apr 21 19:20:13 2015 +0800

    修改分享，删除无用代码

:100644 100644 4ec78ed... b82f668... M  README.md

commit 28a840a5913c1adbf97d25840ad591e85529cd80
Author: xx <xx@xx.com>
Date:   Tue Apr 14 22:44:47 2015 +0800

    first commit

:000000 100644 0000000... 4ec78ed... A  README.md
```

## git log --pretty=oneline filename

可以列出文件的所有改动历史，注意，这是对于一个文件来说的。

```sh
$ git log --pretty=oneline README.md

6c40c0df95b929da59da645ffde0a33f54ff6a3f modify domain
e130233b2eb3d786b7bc36eb90f9560ae751fda9 修改分享，删除无用代码
28a840a5913c1adbf97d25840ad591e85529cd80 first commit
```

## git show

可以使用**git show**显示某个版本的文件修改情况

这个命令和 **git log -p 版本号**一样

```sh
$ git show 6c40c0df95b929da59da645ffde0a33f54ff6a3f

commit 6c40c0df95b929da59da645ffde0a33f54ff6a3f
Author: xx <xx@xx.com>
Date:   Thu May 14 14:08:46 2015 +0800

    modify domain

diff --git a/CNAME b/CNAME
index cfccb43..307b090 100644
--- a/CNAME
+++ b/CNAME
@@ -1 +1 @@
-xx.cn
+vvkee.com
diff --git a/README.md b/README.md
index b82f668..e4f7342 100644
--- a/README.md
+++ b/README.md
@@ -2,4 +2,4 @@

 vvkee的个人博客

-<http://www.xx.cn>
+<http://www.vvkee.com>
```


