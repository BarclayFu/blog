---
title: SpringBoot 3.2.2启动报错 Bean named 'ddlApplicationRunner' is expected to be of type 'org.springframework.boot.Runner' but was actually of type 'org.springframework.beans.factory.support.NullBean'
date: 2024-02-09 19:20:50
tags: 
- Java
- Spring
- mybatis
categories: 后端开发
cover: https://s2.loli.net/2024/02/10/I4ZRmcb7KwV61x9.jpg
---
SpringBoot 3.2.2启动报错 Bean named 'ddlApplicationRunner' is expected to be of type 'org.springframework.boot.Runner' but was actually of type 'org.springframework.beans.factory.support.NullBean'

主要参考了这里->[参考链接](https://github.com/baomidou/mybatis-plus/issues/5867)

解决措施：依赖改成3.5.5