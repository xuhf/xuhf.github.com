---
layout : post
title : "Java使用Base64完成图片与字符串的转换"
category : "java"
tags : [java]
---

## 图片转字符串

将图片的内容读入字节数组中，将字节数组进行Base64编码

使用转换后的字节数组生成字符串

## 字符串转图片

将字符串转换为字节数组，对字节数组进行解码

使用解码后的字节数组生成图片

## 代码

```java
package com.vvkee.jutils.photo;

import java.io.FileInputStream;
import java.io.FileOutputStream;
import java.io.IOException;
import java.io.InputStream;
import java.io.OutputStream;

import org.apache.commons.codec.binary.Base64;
import org.apache.commons.lang.StringUtils;

/**
 * 图像和字符串之间的转换
 *
 * @author vvkee
 *
 */
public class ImageUtil {

    public static void main(String[] args) {
        String imagePath = "F:\\tuoniao.jpg";
        String newImagePath = "F:\\tuoniao_3.jpg";
        String content = toBase64(imagePath);
        System.out.println(content.length());
        System.out.println(content);
        boolean flag = toImage(content, newImagePath);
        if (flag) {
            System.out.println("字符串转图像成功");
        }
    }

    // 将图片文件转化为字节数组字符串，并对其进行Base64编码处理
    public static String toBase64(String imagePath) {
        InputStream in = null;
        byte[] data = null;
        // 读取图片字节数组
        try {
            in = new FileInputStream(imagePath);
            data = new byte[in.available()];
            in.read(data);
            in.close();
        } catch (IOException e) {
            e.printStackTrace();
        }
        // 对字节数组Base64编码
        Base64 base64 = new Base64();
        // 返回Base64编码过的字节数组字符串
        return new String(base64.encode(data));
    }

    // base64字符串转化成图片
    // 对字节数组字符串进行Base64解码并生成图片
    public static boolean toImage(String content, String imagePath) {
        if (StringUtils.isBlank(content) || StringUtils.isBlank(imagePath)) {
            return false;
        }
        Base64 base64 = new Base64();
        byte[] b = content.getBytes();
        try {
            // Base64解码
            b = base64.decode(b);
            for (int i = 0; i < b.length; ++i) {
                // 调整异常数据
                if (b[i] < 0) {
                    b[i] += 256;
                }
            }
            // 生成图片
            OutputStream out = new FileOutputStream(imagePath);
            out.write(b);
            out.flush();
            out.close();
            return true;
        } catch (Exception e) {
            return false;
        }
    }
}
```

## 参考代码

<http://blog.csdn.net/hfhwfw/article/details/5544408>