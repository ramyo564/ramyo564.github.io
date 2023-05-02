---

layout: single
title: " Enhancing the Hello World REST API with a Path Variable "
categories: Spring
tag: [Java,"[BIG] Building REST API with Spring Boot","String.format","@PathVariable"]
toc: true
toc_sticky: true
author_profile: false
sidebar:

---
# Building REST API with Spring Boot (3)

/ @PathVariable /

## Let's understand what is a path parameter
- Path Parameters
	- `/users/{id}/todos/{id}  => /users/2/todos/200`

### HelloWorldController.java
```java
@GetMapping(path = "/hello-world/path-variable/{name}")  
public HelloWorldBean helloWorldPathVariable(@PathVariable String name) {  
	return new HelloWorldBean(String.format("Hello World, %s", name));  
}
```
>- @PathVariable 를 이용해서 파라미터 처리
>- String.format 으로 문자열 형식 지정 
>	- 참고 : https://blog.jiniworld.me/68