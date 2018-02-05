---
layout : post
title : "在SpringBoot中使用SpringDataMongo操作mongo数据库"
category : "java"
tags : [SpringBoot]
published: false
---

SpringBoot版本

```xml
<parent>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-parent</artifactId>
    <version>1.3.2.RELEASE</version>
</parent>
```

依赖

```xml
<dependencies>
    <dependency>
        <groupId>junit</groupId>
        <artifactId>junit</artifactId>
        <scope>test</scope>
    </dependency>
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-data-mongodb</artifactId>
    </dependency>
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-test</artifactId>
        <scope>test</scope>
    </dependency>
</dependencies>
```

连接信息配置

```
spring.data.mongodb.uri=mongodb://localhost:27017/api
```

如果是mongo集群（Master-Slaver）

```
spring.data.mongodb.uri=mongodb://127.0.0.1:27017,127.0.0.1:27018/api
```

测试代码

```java
@RunWith(SpringJUnit4ClassRunner.class)
@SpringApplicationConfiguration(classes = App.class)
public class MongoTest {

    @Autowired
    private MongoTemplate mongoTemplate;

    @Test
    public void testInsert() {
        Call call = new Call();
        call.setUser("test");
        call.setParameter("parameter");
        call.setInterfaceCode("api_1");
        call.setStatus("0");
        mongoTemplate.insert(call, "t_user_log");
    }

    @Test
    public void testInsertMany() {
        Call call = new Call();
        call.setUser("aaa");
        call.setParameter("sss");
        call.setInterfaceCode("3");
        call.setStatus("0");

        Call call2 = new Call();
        call2.setUser("bbb");
        call2.setParameter("dddd");
        call2.setInterfaceCode("4");
        call2.setStatus("0");
        List<Call> calls = new ArrayList<Call>();
        calls.add(call);
        calls.add(call2);
        mongoTemplate.insert(calls, "t_user_log");
    }

    @Test
    public void find() {
        Query query = new Query();
        query.addCriteria(Criteria.where("status").is("0"));
        List<Call> calls = mongoTemplate.find(query, Call.class, "t_user_log");
        for (Call call : calls) {
            System.out.println(call.toString());
        }
    }

    public static class Call {
        private String user;
        private String parameter;
        private String interfaceCode;
        private String status;

        public String getUser() {
            return user;
        }

        public void setUser(String user) {
            this.user = user;
        }

        public String getParameter() {
            return parameter;
        }

        public void setParameter(String parameter) {
            this.parameter = parameter;
        }

        public String getInterfaceCode() {
            return interfaceCode;
        }

        public void setInterfaceCode(String interfaceCode) {
            this.interfaceCode = interfaceCode;
        }

        public String getStatus() {
            return status;
        }

        public void setStatus(String status) {
            this.status = status;
        }

        @Override
        public String toString() {
            return "Call{" +
                    "user='" + user + '\'' +
                    ", parameter='" + parameter + '\'' +
                    ", interfaceCode='" + interfaceCode + '\'' +
                    ", status='" + status + '\'' +
                    '}';
        }
    }

}
```