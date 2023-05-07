---

layout: single
title: " [Spring] Versioning REST API "
categories: Spring
tag: [Java,"[BIG][Spring] Advanced REST API Features","[Spring] Versioning REST API","[Spring] API 버전관리"]
toc: true
toc_sticky: true
author_profile: false
sidebar:

---
# Advanced REST API Features (4)

/ Versioning REST API /

- 유저가 계속해서 바꿔야 하는 API는 좋지 않다
- 고객들이 준비되면 새로운 버전을 사용할 수 있게 버전을 나눠서 관리해야한다.

## Versionin REST API
- You have build an amazing REST API
	- You have 100s of consumers
	- You need to implement a breaking change
		- Example : Split name into firstName and lastName
- SOLUTION : Versioning REST API
	- Variety of options
		- URL
		- Request Parameter
		- Header
		- Media Type

## VersioningPersonController.java
```java
package com.in28minutes.rest.webservices.restfulwebservices.versioning;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RestController;
@RestController
public class VersioningPersonController {
    @GetMapping("/v1/person")
    public PersonV1 getFirstVersionOfPerson(){
        return new PersonV1("Jack Jackson");
    }
    @GetMapping("/v2/person")
    public PersonV2 getSecondVersionOfPerson(){
        return new PersonV2(new Name("Jack","Jackson"));
    }
}
```
>- 우선 url 주소를 분리해준다.
>- Person2를 통해 이름을 분리해서 받는다.

## PersonV1.java
```java
package com.in28minutes.rest.webservices.restfulwebservices.versioning;
public class PersonV1 {
    private String name;
    public PersonV1(String name) {
        this.name = name;
    }
    public String getName() {
        return name;
    }
    @Override
    public String toString() {
        return "PersonV1{" +
                "name='" + name + '\'' +
                '}';
    }
}
```

## PersonV2.java
```java
package com.in28minutes.rest.webservices.restfulwebservices.versioning;
public class PersonV2 {
    private Name name;
    public PersonV2(Name name) {
        this.name = name;
    }
    public Name getName() {
        return name;
    }
    @Override
    public String toString() {
        return "PersonV2{" +
                "name=" + name +
                '}';
    }
}
```
>- PersonV2 에서는 성과 이름을 분리해서 받으므로 class Name을 만들어준다
>- 그리고 Name 으로 만든 객체 name으로 작업하면 된다.
>- "name=" 이부분이 파이썬이랑 은근 헷갈 ... 

### Name.java
```java
package com.in28minutes.rest.webservices.restfulwebservices.versioning;
public class Name {
    private String firstName;
    private String lastName;
    public Name(String firstName, String lastName) {
        this.firstName = firstName;
        this.lastName = lastName;
    }
    public String getFirstName() {
        return firstName;
    }
    public String getLastName() {
        return lastName;
    }
    @Override
    public String toString() {
        return "Name{" +
                "firstName='" + firstName + '\'' +
                ", lastName='" + lastName + '\'' +
                '}';
    }
}
```


## Ver 1
![](https://i.imgur.com/RS0ltrI.png)

## Ver 2
![](https://i.imgur.com/7wlOX9h.png)
