---
layout : post
title : "SpringBoot配置tomcat临时文件目录"
category : "Java"
tags : [Springboot,tomcat,临时目录]
---


## 问题

在使用Springboot的过程中，将Springboot部署在linux的服务器上.

在有一些时候，就会出现tomcat临时目录的问题，那么，应该如何解决这个问题呢？

## 解决

在`application.yaml`中增加如下配置项

```
server：
  tomcat：
    basedir：自定义路径
```

或者在`application.propperties`中增加如下配置项

```
server.tomcat.basedir=自定义路径
```

并且可修改上传文件临时目录

```java
@Bean
MultipartConfigElement multipartConfigElement() {
    String tempPath = "自定义路径";
    MultipartConfigFactory factory = new MultipartConfigFactory();
    factory.setLocation(tempPath);
    return factory.createMultipartConfig();
}
```
