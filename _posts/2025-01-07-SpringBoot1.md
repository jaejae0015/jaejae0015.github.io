---
title: Spring Boot 기본 CRUD와 Mybatis(Mysql)
author:
  name: jaejae0015
  link: https://github.com/jaejae0015
date: 2025-01-07
categories: [Developer, JECKJECK]
math: true
mermaid: true
render_with_liquid: false
pin: false
comments: true
#image:
#  src: /commons/devices-mockup.png
#  width: 800
#  height: 500
---

1.org.apache.ibatis.binding.BindingException: Invalid bound statement (not found):   
Spring Application에 @MapperScan을 잘 해두었는지 확인하자.

2.MyBatis 연결 시 아래와 같은 오류 발생   
***************************   
APPLICATION FAILED TO START   
***************************   
Consider defining a bean of type 'com.moddk.swagger.mapper.TodoMapper' in your configuration.   
   
pom.xml에 Mybatis 버전을 확인하자   
```
<dependency>
  <groupId>org.mybatis.spring.boot</groupId>
  <artifactId>mybatis-spring-boot-starter</artifactId>
  <version>3.0.3</version>
</dependency>
```
   
3.JSP 파일을 이용할 때, suffix/prefix가 동작하지 않는다.   
=> Controller소스에 @RestController가 아닌  @Controller로 명시해야 한다.   
   

---