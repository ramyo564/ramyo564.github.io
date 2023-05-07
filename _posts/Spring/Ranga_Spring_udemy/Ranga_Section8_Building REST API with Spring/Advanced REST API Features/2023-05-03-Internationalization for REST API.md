---

layout: single
title: " [Spring] Internationalization for REST API "
categories: Spring
tag: [Java,"[BIG][Spring] Advanced REST API Features","[Spring] MessageSource","[Spring] i18n"]
toc: true
toc_sticky: true
author_profile: false
sidebar:

---
# Advanced REST API Features (3)

/ i18n / MessageSource

## Internationalization for REST API
- Your REST API might have consumers from around the world
- How do you customize it to users around the world?
	- Internationalization - i18n
- Typically HTTP Request Header - Accect-Language is used
	- Accept-Language - indicates natural language and locale that the consumer prefers
	- Example: en - English
	- Example: nl - Dutch
	- Example: kr - Korean

## messages.properties
```java
good.morning.message=Good Morning
```
>- 반드시 resources 안에 있어야 한다.
## messages_kr.properties
```java
good.morning.message=굿모닝
```

### HelloWorldController.java
```java
@RestController
public class HelloWorldController {

    private MessageSource messageSource;
    public HelloWorldController(MessageSource messageSource){
        this.messageSource = messageSource;
    }
    
    @GetMapping(path = "/hello-world-internationalized")
    public String HelloWorldInternationalized(){
        Locale locale = LocaleContextHolder.getLocale();
        return messageSource.getMessage(
                "good.morning.message",
                null,
                "Default Message",
                locale
        );

    }
}
```
>- 스프링 메세지 소스(Spring MeesageSource)는 국제화(i18n)을 제공하는 인터페이스다. 메세지 설정 파일을 모아놓고 각 국가마다 로컬라이징을 함으로서 쉽게 각 지역에 맞춘 메세지를 제공할 수 있다. 
>- 참고 : https://engkimbs.tistory.com/717

### 디폴트
![](https://i.imgur.com/U1iKkIU.png)

### 헤더에 kr 추가
![](https://i.imgur.com/yO8Ctux.png)
