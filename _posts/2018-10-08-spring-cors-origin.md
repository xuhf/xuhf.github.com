---
layout : post
title : "在SpringBoot中配置跨域"
category : "java"
tags : [springboot]
---

在启动类中添加如下

```java
/**
 * 跨域过滤器
 *
 * @return
 */
@Bean
public CorsFilter corsFilter() {
    CorsConfiguration corsConfiguration = new CorsConfiguration();
    corsConfiguration.addAllowedOrigin("*");
    corsConfiguration.addAllowedHeader("*");
    corsConfiguration.addAllowedMethod("*");
    UrlBasedCorsConfigurationSource source = new UrlBasedCorsConfigurationSource();
    source.registerCorsConfiguration("/api/**", corsConfiguration);
    return new CorsFilter(source);
}
```
