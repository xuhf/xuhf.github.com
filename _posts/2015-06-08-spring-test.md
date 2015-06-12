---
layout : post
title : "使用Spring进行单元测试"
category : "java"
keywords : spring,test
tags : [java]
---

## 测试的作用

作为一个开发人员，我们为什么要写单元测试？

  1. 我们可以通过编写单元测试来验证自己程序的有效性
  2. 通过持续自动的执行单元测试和分析单元测试的覆盖率等来确保软件本身的质量

## 当Spring遇见测试

```java
@ContextConfiguration(locations = { "classpath:applicationContext.xml", "classpath:spring-mybatis.xml" })
@RunWith(SpringJUnit4ClassRunner.class)
@Transactional
public class MemberServiceTest {

  @Autowired
  private MemberService memberService;

}
```

  1. @ContextConfiguration 指定Spring的配置文件位置。
  2. @RunWith(SpringJUnit4ClassRunner.class) 启动Spring对测试类的支持。
  3. @Transactional 表明此测试类的事务启用，这样所有的测试方案都会自动的 rollback。

## 参考链接

<http://www.ibm.com/developerworks/cn/java/j-lo-springunitest/>
