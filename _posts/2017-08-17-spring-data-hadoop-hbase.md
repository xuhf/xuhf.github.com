---
layout : post
title : "使用Spring-data-hadoop配置Hbase"
category : "java"
tags : [hbase]
---

## 依赖

```xml
<dependency>
    <groupId>org.springframework.data</groupId>
    <artifactId>spring-data-hadoop</artifactId>
    <version>2.4.0.RELEASE</version>
</dependency>
<dependency>
    <groupId>org.apache.hbase</groupId>
    <artifactId>hbase-client</artifactId>
    <version>1.2.1</version>
</dependency>
```

## 配置

在这里需要2个配置文件，一个是`spring-hbase.xml`，一个是`hbase-site.xml`

spring-hbase.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:hdp="http://www.springframework.org/schema/hadoop"
    xmlns:p="http://www.springframework.org/schema/p"
    xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans-4.2.xsd http://www.springframework.org/schema/hadoop http://www.springframework.org/schema/hadoop/spring-hadoop-2.3.xsd">

    <!-- HDFS配置 -->
    <hdp:configuration resources="classpath:/hbase-site.xml" />
    <!-- HBase连接配置 -->
    <hdp:hbase-configuration configuration-ref="hadoopConfiguration" />
    <!-- HbaseTemplate Bean配置 -->
    <bean id="hbaseTemplate" class="org.springframework.data.hadoop.hbase.HbaseTemplate">
        <property name="configuration" ref="hbaseConfiguration"></property>
        <property name="encoding" value="UTF-8"></property>
    </bean>

</beans>
```

hbase-site.xml

```xml
<?xml version="1.0"?>
<?xml-stylesheet type="text/xsl" href="configuration.xsl"?>
<configuration>
    <property>
        <name>hbase.zookeeper.quorum</name>
        <value>127.0.0.1</value>
    </property>
    <property>
        <name>hbase.rpc.timeout</name>
        <value>300000</value>
    </property>
</configuration>
```

## 测试代码

```
import java.util.Map;

import org.apache.hadoop.hbase.KeyValue;
import org.apache.hadoop.hbase.client.Result;
import org.apache.hadoop.hbase.client.ResultScanner;
import org.apache.hadoop.hbase.client.Scan;
import org.apache.hadoop.hbase.filter.FirstKeyOnlyFilter;
import org.apache.hadoop.hbase.util.Bytes;
import org.springframework.context.support.AbstractApplicationContext;
import org.springframework.context.support.ClassPathXmlApplicationContext;
import org.springframework.data.hadoop.hbase.HbaseTemplate;
import org.springframework.data.hadoop.hbase.ResultsExtractor;
import org.springframework.data.hadoop.hbase.RowMapper;

import com.google.common.collect.Maps;

public class Main {

    public static void main(String[] args) throws Exception {
        AbstractApplicationContext context = new ClassPathXmlApplicationContext("applicationContext.xml");
        HbaseTemplate template = context.getBean(HbaseTemplate.class);
        template.getConfiguration();
        Map<String, Object> map = template.get("t_table", "row1",new RowMapper<Map<String, Object>>() {
            @SuppressWarnings("deprecation")
            public Map<String, Object> mapRow(Result r, int rowNum) throws Exception {
                Map<String, Object> result = Maps.newHashMap();
                if (r.isEmpty()) {
                    return result;
                }
                for (KeyValue kv : r.list()) {
                    result.put(Bytes.toString(kv.getQualifier()), Bytes.toString(kv.getValue()));
                }
                return result;
            }
        });
        System.out.println(map);

        // 统计表中行数
        Scan scan = new Scan();
        scan.setFilter(new FirstKeyOnlyFilter());
        long count = template.find("t_table", scan, new ResultsExtractor<Long>() {
            public Long extractData(ResultScanner results) throws Exception {
                long rowCount = 0;
                for (Result result : results) {
                    rowCount += result.size();
                }
                return rowCount;
            }
        });
        System.out.println(count);
        context.close();
    }
}
```


## 存在问题

如果你自己的项目使用了google的guava包，那么可能会存在jar包冲突的问题

我自己也没有找到解决办法，

只是将guava改到统一的版本


