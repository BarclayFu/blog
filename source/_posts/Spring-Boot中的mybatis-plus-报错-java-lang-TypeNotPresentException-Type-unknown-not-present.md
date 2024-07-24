---
title: >-
  Spring Boot中的mybatis-plus 报错 java.lang.TypeNotPresentException: Type [unknown]
  not present
date: 2024-02-09 18:42:24
tags: 
- Spring
- mybatis
- Java
categories: 后端开发
cover: https://s2.loli.net/2024/02/10/I4ZRmcb7KwV61x9.jpg
---

这里主要是参考了[这个博客](https://www.cnblogs.com/nosouln/p/12805603.html)
```xml
<dependency>
  <groupId>com.baomidou</groupId>
  <artifactId>mybatis-plus</artifactId>
  <version>3.5.5</version>
</dependency>
```
加一个后缀boot-starter

```xml
<dependency>
  <groupId>com.baomidou</groupId>
  <artifactId>mybatis-plus-boot-starter</artifactId>
  <version>3.5.5</version>
</dependency>
```