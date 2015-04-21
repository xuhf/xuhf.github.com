---
title : SpringMVC-Controller中使用JSON格式数据入参
layout : post
category : [springmvc,json]
---

## 需求

最近在最一个接口，接口需要接收一个json格式的数据入参，之前使用的是参数名加参数值的方式

```java
@RequestMapping(value = "submit", method = RequestMethod.POST)
@ResponseBody
public String payment(@RequestParam(value = "d")
String d, HttpServletRequest request) {
}
```
但是这样就会造成一个问题，就必须指定参数名，否则就接收不到参数。

那么就需要对接口进行修改。

## 解决

在spring-mvc.xml中增加内容

```xml
<!--  使用json格式的数据作为参数 -->
<bean
  class="org.springframework.web.servlet.mvc.method.annotation.RequestMappingHandlerAdapter">
  <property name="messageConverters">
    <list>
      <ref bean="jsonMessageConverter" />
    </list>
  </property>
</bean>

<bean id="jsonMessageConverter"
  class="org.springframework.http.converter.json.MappingJackson2HttpMessageConverter">
</bean>
```
接下来我们就可以使用@RequestBody来标记参数。

ParameterModel 需要和你入参的json格式保持一致

```java
@RequestMapping(value = "submit", method = RequestMethod.POST, consumes = "application/json")
@ResponseBody
public AjaxJsonResponse payment(@RequestBody ParameterModel parameterModel,
    HttpServletRequest request) {

}
```
Jackson的依赖

```xml
<dependency>
  <groupId>com.fasterxml.jackson.core</groupId>
  <artifactId>jackson-core</artifactId>
  <version>2.4.4</version>
</dependency>
<dependency>
  <groupId>org.codehaus.jackson</groupId>
  <artifactId>jackson-mapper-asl</artifactId>
  <version>1.9.13</version>
</dependency>
<dependency>
  <groupId>com.fasterxml.jackson.core</groupId>
  <artifactId>jackson-databind</artifactId>
  <version>2.2.3</version>
</dependency>
```

## 参考链接

[http://www.journaldev.com/2552/spring-restful-web-service-example-with-json-jackson-and-client-program](http://www.journaldev.com/2552/spring-restful-web-service-example-with-json-jackson-and-client-program)
