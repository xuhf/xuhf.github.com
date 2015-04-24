---
layout : post
title : "Unicode转汉字"
category : "java"
tags : [unicode]
---

## 前言

在项目里我们会遇到unicode需要转换成汉字的需求。留下代码供未来使用。

## 代码

```java
import java.nio.ByteBuffer;
import java.nio.charset.Charset;
import java.util.regex.Matcher;
import java.util.regex.Pattern;
public class UnicodeUtils {
  public static String decodeUnicode(String str) {
    Charset set = Charset.forName("UTF-16");
    Pattern p = Pattern.compile("\\\\u([0-9a-fA-F]{4})");
    Matcher m = p.matcher(str);
    int start = 0;
    int start2 = 0;
    StringBuffer sb = new StringBuffer();
    while (m.find(start)) {
      start2 = m.start();
      if (start2 > start) {
        String seg = str.substring(start, start2);
        sb.append(seg);
      }
      String code = m.group(1);
      int i = Integer.valueOf(code, 16);
      byte[] bb = new byte[4];
      bb[0] = (byte) ((i >> 8) & 0xFF);
      bb[1] = (byte) (i & 0xFF);
      ByteBuffer b = ByteBuffer.wrap(bb);
      sb.append(String.valueOf(set.decode(b)).trim());
      start = m.end();
    }
    start2 = str.length();
    if (start2 > start) {
      String seg = str.substring(start, start2);
      sb.append(seg);
    }
    return sb.toString();
  }
  public static void main(String[] args) {
    String title = "\u67d0\u5927\u578b\u5728\u7ebf\u5b66\u4e60\u5e73\u53f0\u7ba1\u7406\u9875\u9762\u672a\u6388\u6743\u8bbf\u95ee\u6216\u53efxss";
    System.out.println(decodeUnicode(title));
  }
}
```
