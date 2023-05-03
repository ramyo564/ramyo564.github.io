---

layout: single
title: " Creating a Hello World REST API with Spirng Boot "
categories: Spring
tag: [Java,"[BIG] Building REST API with Spring Boot","@GetMapping","@RestController","@RequestMapping","dispatcher servlet"]
toc: true
toc_sticky: true
author_profile: false
sidebar:

---
# Building REST API with Spring Boot (2)

/ @RestController / @RequestMapping / @GetMapping / dispatcher servlet

## Creating a Hello World REST API with Spring Boot

```java
@RestController  
public class HelloWorldController {  
  
@GetMapping(path = "/hello-world")  
public String helloWorld() {  
	return "Hello World";  
}
```
>- @RestController 을 통해서 컨트롤러를 등록하고 @RequestMapping 통해서 API URL을 매핑 시킬 수 있다.
>- @RequestMapping 을 사용할 경우 메소드를 정해줘야 한다.
>	- `@RequestMapping(method = RequestMethod.GET, path = "/hello-world") `
>- 하지만 @GetMapping을 사용하면 조금 더 간결하게 사용 가능하다.

## Enhancing the Hello World REST API to return a Bean
- How to return a JSON back from the rest API in this step

### HelloWorldBean.java
```java
public class HelloWorldBean {  
private String message;  
public HelloWorldBean(String message) {  
this.message = message;  
}  
public String getMessage() {  
return message;  
}  
public void setMessage(String message) {  
this.message = message;  
}  
@Override  
public String toString() {  
return "HelloWorldBean [message=" + message + "]";  
}  
}
```

### HelloWorldController.java
```java
@RestController  
public class HelloWorldController {  
@GetMapping(path = "/hello-world")  
public String helloWorld() {  
return "Hello World";  
}  
@GetMapping(path = "/hello-world-bean")  
public HelloWorldBean helloWorldBean() {  
return new HelloWorldBean("Hello World");  
}  
}
```


![](https://i.imgur.com/VSgpnSy.png)
>- 더 이상 단순 String 이 아니라 JSON 으로 반환되어 출력된다.
>- 근데 이게 어떻게 되는걸까?

## What's Happening in the Background
- Let's exploring some Spring Boot Magic : Enable Debug Logging
	- WARNING : Log change frequently! 
	- `logging.level.org.springframework=debug` <-application.properties
- ***How are our requests handled?***
	- DispatcherServlet - Front Controller Pattern
		- All requests when we talk about spring, MVC are being handled by dispatcher servlet. this is called different controller pattern
			- All the requests first go to the dispatcher servlet
		- Mapping servlets : dispatcherServlet urls = `[/]`
			- dispatcher servlet is mapped to the root URL
			- If you search in the console logs and if you search for mapping servlet or dispatcher servlet.
			- If you search the logs, you should see something called mapping servlet in here.
		- Auto Configuration (DispatcherServletAutoConfiguration)
			- Auto configuration is one of the most important springboard features based on the classes which are available in the class path. Spring Boot would automatically detect the fact that we are building a web application or rest API and therefore it automatically configures a dispatcher servlet.
			- If you search for dispatch servlet auto configuration in the logs, if you search for dispatch servlet auto configuration in the log, you should see a bean which is being created. So this has matched and therefore this would go ahead and configure a dispatcher servlet for us.
- ***How does HelloWorldBean object get converted to JSON?***
	- @ResponseBody + JacksonHttpMessageConverters
		- Auto Configuration(JacksonHttpMessageConvertersConfiguration)
- ***Who is configuring error mapping?***
	- Auto Configuration (ErrorMvcAutoConfiguration)
- ***How are all jars available(Spring, SpringMVC, Jackson, Tomcat)?***
	- Starter Projects - Spring Boot Starter Web (spring-webmvcm, spring-web, spring-boot-starter-tomcat, spring-boot-starter-json)
