---
title : MybatisGenerator的使用
layout : post
category : mybatis
---

## 简介

以下简介来自这里:[http://generator.sturgeon.mopaas.com/index.html](http://generator.sturgeon.mopaas.com/index.html)

MyBatis Generator (MBG) 是一个Mybatis的代码生成器 MyBatis 和 iBATIS. 他可以生成Mybatis各个版本的代码，和iBATIS 2.2.0版本以后的代码。 他可以内省数据库的表（或多个表）然后生成可以用来访问（多个）表的基础对象。 这样和数据库表进行交互时不需要创建对象和配置文件。 MBG的解决了对数据库操作有最大影响的一些简单的CRUD（插入，查询，更新，删除）操作。 您仍然需要对联合查询和存储过程手写SQL和对象。

## 如何使用

### 第一步

因为我是使用maven来构建项目,所以在src/main/resources下新建generatorConfig.xml文件,将以下内容copy到xml文件中.

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE generatorConfiguration
  PUBLIC "-//mybatis.org//DTD MyBatis Generator Configuration 1.0//EN"
  "http://mybatis.org/dtd/mybatis-generator-config_1_0.dtd">

<generatorConfiguration>

  <!-- JDBC -->
  <classPathEntry
    location="/Users/xuhaifei/.m2/repository/mysql/mysql-connector-java/5.1.26/mysql-connector-java-5.1.26.jar" />
  <context id="CORE" targetRuntime="MyBatis3">
    <!-- comment -->
    <commentGenerator>
      <property name="suppressAllComments" value="true" />
      <property name="suppressDate" value="true" />
    </commentGenerator>

    <!-- mysql -->
    <jdbcConnection driverClass="com.mysql.jdbc.Driver"
      connectionURL="jdbc:mysql://localhost:3306/mybatis?characterEncoding=utf8"
      userId="root" password="password">
    </jdbcConnection>

    <javaTypeResolver>
      <property name="forceBigDecimals" value="false" />
    </javaTypeResolver>

    <!-- PO -->
    <javaModelGenerator targetPackage="test.po"
      targetProject="src/main/java">
      <property name="enableSubPackages" value="true" />
      <property name="trimStrings" value="true" />
    </javaModelGenerator>

    <!-- xml -->
    <sqlMapGenerator targetPackage="test.dao"
      targetProject="src/main/resources">
      <property name="enableSubPackages" value="true" />
    </sqlMapGenerator>

    <!-- DAO -->
    <javaClientGenerator type="XMLMAPPER"
      targetPackage="test.dao" targetProject="src/main/java">
      <property name="enableSubPackages" value="true" />
    </javaClientGenerator>

    <!-- table -->
    <!-- 这需要注意,将你需要自动生成的表都列在这里 -->
    <table tableName="test" schema="public"
      domainObjectName="Test" />

  </context>

</generatorConfiguration>
```
### 第二步

在maven的pom.xml文件中增加以下内容 **如果你的pom文件中已经存在其他插件,就只增加plugin标签下的东西就可以了**

```xml
<build>
  <plugins>
    <plugin>
      <groupId>org.mybatis.generator</groupId>
      <artifactId>mybatis-generator-maven-plugin</artifactId>
      <version>1.3.2</version>
      <configuration>
        <verbose>true</verbose>
        <overwrite>true</overwrite>
      </configuration>
    </plugin>
  </plugins>
</build>
```

### 第三步

在你的项目目录下执行以下命令

```
mvn mybatis-generator:generate
```
