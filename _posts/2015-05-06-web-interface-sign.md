---
layout : post
title : "Web应用接口开发签名方式"
category : "java"
tags : [签名, 接口]
---

## 背景

在进行接口开发时，需要对数据的安全性有一定的要求，尤其是在接口暴露在公网之下，为了防止传输时参数被篡改，我们使用签名sign的方式提高安全性。

## 接口参数签名sign

最初接触这种方式是在淘宝的支付宝接口上，淘宝的支付宝接口都采用的是这种方式，使用合作商户的key对参数进行签名。

生成sign的方法

 1. 将所有参数（sign和sign_type除外,有的时候特定的某一个参数也可以不参加签名,空值不参加签名）按照参数名的字母升序排序。

 2. 将参数值按照 参数名1=参数值1&参数名2=参数值2 的方式拼接成一个字符串

 3. 将密钥key 拼接到 第二步得到的字符串后

 4. 使用MD5或者其他签名方式计算值，得到32位字符串。

计算后的sign值可以都转成大写。

签名验证方法

根据前面描述的签名参数sign生成的方法规则，计算得到参数的签名值，

和参数中通知过来的sign对应的参数值进行对比，如果是一致的，校验通过，如果不一致，说明参数被修改过。

## Java代码实现

### 首先构建参数的Map

```java
Map<String, String> map = new HashMap<String, String>();
map.put("name", "xuhf");
map.put("age", "28");
map.put("site", "http://www.xuhaifei.cn");
map.put("facebook", null);
```

### 剔除空值和不参加签名的参数

```java
public static Map<String, String> filter(Map<String, String> parameters) {
  Map<String, String> result = new HashMap<String, String>();
  if (parameters == null || parameters.size() <= 0) {
    return result;
  }
  for (String key : parameters.keySet()) {
    String value = parameters.get(key);
    if (value == null || value.equals("")
        || key.equalsIgnoreCase("sign")) {
      continue;
    }
    result.put(key, value);
  }
  return result;
}
```

### 对参加签名的参数按照字母升序排序并拼接字符串

```java
public static String buildLinkString(Map<String, String> filterParameters) {
  List<String> keys = new ArrayList<String>(filterParameters.keySet());
  Collections.sort(keys);
  String prestr = "";
  for (int i = 0; i < keys.size(); i++) {
    String key = keys.get(i);
    String value = filterParameters.get(key);
    if (i == keys.size() - 1) {// 拼接时，不包括最后一个&字符
      prestr = prestr + key + "=" + value;
    } else {
      prestr = prestr + key + "=" + value + "&";
    }
  }
  return prestr;
}
```

### 使用生成的字符串 和 key 生成签名

在获得byte数组时，可以指定字符编码，也可以不指定，

如果指定字符编码，双方要做好约定，或者在参数里携带字符编码。防止验证签名错误。

这里使用了 commons-codec-1.9.jar

```java
public static String sign(String key, String prestr) throws UnsupportedEncodingException {
  prestr = prestr + key;
  String charset = "UTF-8";
  byte[] b = prestr.getBytes(charset);
  String mysign = DigestUtils.md5Hex(b);
  return mysign;
}
```

## 完整代码

```java
import java.io.UnsupportedEncodingException;
import java.util.ArrayList;
import java.util.Collections;
import java.util.HashMap;
import java.util.List;
import java.util.Map;

import org.apache.commons.codec.digest.DigestUtils;

public class SignDemo {

  public static void main(String[] args) throws UnsupportedEncodingException {
    sign();
  }

  public static void sign() throws UnsupportedEncodingException {
    Map<String, String> parameters = new HashMap<String, String>();
    parameters.put("name", "xuhf");
    parameters.put("age", "28");
    parameters.put("site", "http://www.xuhaifei.cn");
    parameters.put("facebook", null);

    Map<String, String> filterParameters = filter(parameters);

    String s = buildLinkString(filterParameters);

    String key = "java";

    String sign = sign(key, s);

    System.out.println(sign);

  }

  public static Map<String, String> filter(Map<String, String> parameters) {
    Map<String, String> result = new HashMap<String, String>();
    if (parameters == null || parameters.size() <= 0) {
      return result;
    }
    for (String key : parameters.keySet()) {
      String value = parameters.get(key);
      if (value == null || value.equals("")
          || key.equalsIgnoreCase("sign")) {
        continue;
      }
      result.put(key, value);
    }
    return result;
  }

  public static String buildLinkString(Map<String, String> filterParameters) {
    List<String> keys = new ArrayList<String>(filterParameters.keySet());
    Collections.sort(keys);
    String prestr = "";
    for (int i = 0; i < keys.size(); i++) {
      String key = keys.get(i);
      String value = filterParameters.get(key);
      if (i == keys.size() - 1) {// 拼接时，不包括最后一个&字符
        prestr = prestr + key + "=" + value;
      } else {
        prestr = prestr + key + "=" + value + "&";
      }
    }
    return prestr;
  }

  public static String sign(String key, String prestr)
      throws UnsupportedEncodingException {
    prestr = prestr + key;
    String charset = "UTF-8";
    byte[] b = prestr.getBytes(charset);
    String mysign = DigestUtils.md5Hex(b);
    return mysign;
  }
}
```

## 参考文章

<http://www.oicto.com/web-api-sign-key-a/>
<http://www.oicto.com/web-api-sign-key-b/>
