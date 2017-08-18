---
layout : post
title : "使用Spring-data-redis 操作Redis Cluster"
category : "java"
tags : [redis,spring-data-redis, redis-cluster]
---

## Redis版本

redis cluster 使用`redis-3.2.5`

## 环境配置

在`spring-data-redis 1.7`之后，就增加了对`cluster`的支持，所以在这里我们选择`1.8.6.RELEASE`

相应的，`jedis`的版本选择`2.9.0`

`pom.xml`

```xml
<dependency>
    <groupId>org.springframework.data</groupId>
    <artifactId>spring-data-redis</artifactId>
    <version>1.8.6.RELEASE</version>
</dependency>
<dependency>
    <groupId>redis.clients</groupId>
    <artifactId>jedis</artifactId>
    <version>2.9.0</version>
</dependency>
<!--在测试时使用-->
<dependency>
    <groupId>junit</groupId>
    <artifactId>junit</artifactId>
    <version>4.12</version>
    <scope>test</scope>
</dependency>
<dependency>
    <groupId>org.springframework</groupId>
    <artifactId>spring-test</artifactId>
    <version>4.2.5.RELEASE</version>
</dependency>
```

`spring-redis.xml`

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:p="http://www.springframework.org/schema/p"
    xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans-4.2.xsd">
    <description>Jedis Cluster Configuration</description>

    <bean id="jedisPoolConfig" class="redis.clients.jedis.JedisPoolConfig">
        <property name="maxIdle" value="100" />
        <property name="maxTotal" value="600" />
        <property name="maxWaitMillis" value="10000" />
        <property name="testOnBorrow" value="true" />
        <property name="testOnReturn" value="true" />
    </bean>

    <!-- 配置Cluster -->
    <bean id="redisClusterConfiguration"
        class="org.springframework.data.redis.connection.RedisClusterConfiguration">
        <property name="maxRedirects" value="3"></property>
        <!-- 节点配置 -->
        <property name="clusterNodes">
            <set>
                <bean class="org.springframework.data.redis.connection.RedisClusterNode">
                    <constructor-arg name="host" value="127.0.0.1"></constructor-arg>
                    <constructor-arg name="port" value="6379"></constructor-arg>
                </bean>
                <bean class="org.springframework.data.redis.connection.RedisClusterNode">
                    <constructor-arg name="host" value="127.0.0.1"></constructor-arg>
                    <constructor-arg name="port" value="6380"></constructor-arg>
                </bean>
                <bean class="org.springframework.data.redis.connection.RedisClusterNode">
                    <constructor-arg name="host" value="127.0.0.1"></constructor-arg>
                    <constructor-arg name="port" value="6381"></constructor-arg>
                </bean>
            </set>
        </property>
    </bean>

    <bean id="jeidsConnectionFactory"
        class="org.springframework.data.redis.connection.jedis.JedisConnectionFactory">
        <constructor-arg ref="redisClusterConfiguration" />
        <constructor-arg ref="jedisPoolConfig" />
    </bean>

    <!-- redis 访问的模版 -->
    <bean id="redisTemplate" class="org.springframework.data.redis.core.RedisTemplate">
        <property name="connectionFactory" ref="jeidsConnectionFactory" />
    </bean>
</beans>
```

## 测试

`JUnit4ClassRunner.java`

```java
import java.io.FileNotFoundException;

import org.junit.runners.model.InitializationError;
import org.springframework.test.context.junit4.SpringJUnit4ClassRunner;
import org.springframework.util.Log4jConfigurer;

public class JUnit4ClassRunner extends SpringJUnit4ClassRunner {
    static {
        try {
            Log4jConfigurer.initLogging("classpath:log4j.properties");
        } catch (FileNotFoundException ex) {
            System.err.println("Cannot Initialize log4j");
        }
    }

    public JUnit4ClassRunner(Class<?> clazz) throws InitializationError {
        super(clazz);
    }
}
```

`Test.java`

```java
import java.util.Set;

import org.junit.runner.RunWith;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.dao.DataAccessException;
import org.springframework.data.redis.connection.RedisConnection;
import org.springframework.data.redis.core.RedisCallback;
import org.springframework.data.redis.core.RedisTemplate;
import org.springframework.test.context.ContextConfiguration;

import com.google.common.collect.Sets;

@RunWith(JUnit4ClassRunner.class)
@ContextConfiguration(locations = "classpath:spring-redis.xml")
public class Test {

    @Autowired
    private RedisTemplate<String, String> redisTemplate;

    @org.junit.Test
    public void get() {
        final String key = "vvkee.password";
        String v = redisTemplate.execute(new RedisCallback<String>() {
            public String doInRedis(RedisConnection connection) throws DataAccessException {
                return new String(connection.get(key.getBytes()));
            }
        });
        System.out.println(v);
    }

    @org.junit.Test
    public void set() {
        final String key = "vvkee";
        final String value = "http://www.vvkee.com";
        boolean flag = redisTemplate.execute(new RedisCallback<Boolean>() {
            public Boolean doInRedis(RedisConnection connection) throws DataAccessException {
                try {
                    connection.set(key.getBytes(), value.getBytes());
                    return true;
                } catch (Exception e) {
                    return false;
                }
            }
        });
        System.out.println(flag);
    }

    @org.junit.Test
    public void setEx() {
        final String key = "vvkee_90";
        final String value = "www.vvkee.com";
        final long seconds = 90;
        boolean flag = redisTemplate.execute(new RedisCallback<Boolean>() {
            public Boolean doInRedis(RedisConnection connection) throws DataAccessException {
                try {
                    connection.setEx(key.getBytes(), seconds, value.getBytes());
                    return true;
                } catch (Exception e) {
                    return false;
                }
            }
        });
        System.out.println(flag);
    }

    @org.junit.Test
    public void sadd() {
        final String key = "vvkee_interface";
        final String value = "API_5";
        boolean flag = redisTemplate.execute(new RedisCallback<Boolean>() {
            public Boolean doInRedis(RedisConnection connection) throws DataAccessException {
                try {
                    connection.sAdd(key.getBytes(), value.getBytes());
                    return true;
                } catch (Exception e) {
                    return false;
                }
            }
        });
        System.out.println(flag);
    }

    @org.junit.Test
    public void smembers() {
        final String key = "vvkee_interface";
        Set<String> set = redisTemplate.execute(new RedisCallback<Set<String>>() {
            public Set<String> doInRedis(RedisConnection connection) throws DataAccessException {
                Set<String> set = Sets.newHashSet();
                Set<byte[]> members = connection.sMembers(key.getBytes());
                for (byte[] m : members) {
                    set.add(new String(m));
                }
                return set;
            }
        });
        System.out.println(set);
    }
}
```