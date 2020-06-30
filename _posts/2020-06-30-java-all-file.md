---
layout : post
title : "Java获取目录下所有文件"
category : "Java"
tags : [Java,文件,递归]
---

## 需求

在很多时候，我们需要根据目录，获取目录下所有文件，在这个时候，我们就可以考虑使用递归的方式获取所有文件。

并根据文件夹和文件名进行排序。

## 代码

```java
import org.apache.commons.io.FileUtils;

import java.io.File;
import java.io.IOException;
import java.util.Arrays;
import java.util.Collections;
import java.util.Comparator;
import java.util.List;

public static void getAllFiles(File dir, List<String> files) {
    if (dir.isDirectory()) {
        List<File> list = Arrays.asList(dir.listFiles());
        Collections.sort(list, new Comparator<File>() {
            @Override
            public int compare(File o1, File o2) {
                if (o1.isDirectory() && o2.isFile())
                    return -1;
                if (o1.isFile() && o2.isDirectory())
                    return 1;
                return o1.getName().compareTo(o2.getName());
            }
        });
        for (File f : list) {
            //如果这个文件是目录，则进行递归搜索
            if (f.isDirectory()) {
                getAllFiles(f, files);
            } else {
                // 删除隐藏文件
                if (f.isHidden()) {
                    try {
                        FileUtils.forceDelete(f);
                    } catch (IOException e) {
                        e.printStackTrace();
                    }
                } else {
                    //如果文件是普通文件，则将文件句柄放入集合中
                    files.add(f.getAbsolutePath());
                }
            }
        }
    }
}
```