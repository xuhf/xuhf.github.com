---
layout : post
title : "Java读取配置文件"
category : "java"
tags : [java,properties]
---

在系统开发中，或多或少的使用配置文件来管理系统运行时的一些参数。

**\*\.properties**是我们使用的比较多的一种方式。

## 使用java.util.Properties类的load方法

示例代码如下

```java
import java.io.IOException;
import java.io.InputStream;
import java.io.InputStreamReader;
import java.util.Properties;

public class PropertiesLoader {

  private static Properties properties = new Properties();

  static {
    try {
      InputStream in = Thread.currentThread().getContextClassLoader().getResourceAsStream("demo.properties");
      // 强烈建议加上编码
      properties.load(new InputStreamReader(in, "UTF-8"));
    } catch (IOException e) {
      throw new RuntimeException("load demo.properties file error");
    }
  }

  public static void main(String[] args) {
    System.out.println(properties.getProperty("appKey"));
  }
}
```

## 使用apache-commons-configuration包

虽然上面的方式可以实现我们的目的，但是 **properties.getProperty("appKey")** 获得到的属性都是String类型的，一点都不酷炫。

并且，我们在项目中，可能还会使用其他类型的配置文件，例如xml、ini

既然有人已经帮我们做好了这些，拿过来用吧（apache-commons系列的包都还不错），需要更为强大功能的同学，请自行前往官网。

在pom.xml文件中增加

```xml
<dependency>
  <groupId>commons-configuration</groupId>
  <artifactId>commons-configuration</artifactId>
  <version>1.10</version>
</dependency>
```

示例代码

```java
import org.apache.commons.configuration.ConfigurationException;
import org.apache.commons.configuration.PropertiesConfiguration;

public class PropertiesLoader {

  public static void main(String[] args) throws ConfigurationException {
    PropertiesConfiguration config = new PropertiesConfiguration("demo.properties");
    System.out.println(config.getString("appId"));
    System.out.println(config.getInteger("vvkee", 1));
  }
}
```
