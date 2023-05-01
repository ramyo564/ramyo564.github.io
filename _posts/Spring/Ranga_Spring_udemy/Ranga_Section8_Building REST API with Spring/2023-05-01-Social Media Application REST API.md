---

layout: single
title: " Social Media Application REST API "
categories: Spring
tag: [Java,"[BIG] Building REST API with Spring Boot",]
toc: true
toc_sticky: true
author_profile: false
sidebar:

---
# Building REST API with Spring Boot (4)

/ !!!!!! /

## Social Media Application REST API
- Build a REST API for a Social Media Application
- Key Resources:
	- Users:
	- Posts
- Key Details:
	- User: id, name, birthDate
	- Post: id, description

### Creating User Bean and UserDaoService

#### User.java
```java
package com.in28minutes.rest.webservices.restfulwebservices.user;  
import java.time.LocalDate;  
public class User {  
private Integer id;  
private String name;  
private LocalDate birthDate;  
  
public User(Integer id, String name, LocalDate birthDate) {  
super();  
this.id = id;  
this.name = name;  
this.birthDate = birthDate;  
}  
  
public Integer getId() {  
return id;  
}  
  
public void setId(Integer id) {  
this.id = id;  
}  
  
public String getName() {  
return name;  
}  
  
public void setName(String name) {  
this.name = name;  
}  
  
public LocalDate getBirthDate() {  
return birthDate;  
}  
  
public void setBirthDate(LocalDate birthDate) {  
this.birthDate = birthDate;  
}  
  
@Override  
public String toString() {  
return "User [id=" + id + ", name=" + name + ", birthDate=" + birthDate + "]";  
}  
  
}
```
>- user에 대한 정보로 아이디랑 이름, 생년월일 만 셋팅하고
>  나머지는 게터세터 등으로처리

#### UserDaoService.java
```java
package com.in28minutes.rest.webservices.restfulwebservices.user;  
  
import java.time.LocalDate;  
import java.util.ArrayList;  
import java.util.List;  
  
import org.springframework.stereotype.Component;  
  
@Component  
public class UserDaoService {  
// JPA/Hibernate > Database  
// UserDaoService > Static List  
  
private static List<User> users = new ArrayList<>();  
  
static {  
users.add(new User(1,"Adam",LocalDate.now().minusYears(30)));  
users.add(new User(2,"Eve",LocalDate.now().minusYears(25)));  
users.add(new User(3,"Jim",LocalDate.now().minusYears(20)));  
}  
  
public List<User> findAll() {  
return users;  
}  
  
//public User save(User user) {  
  
//public User findOne(int id) {  
  
}
```
>- 우선 정적인 데이터로 저장
>- @Component로 전체 클래스를 빈으로 등록

## Request Methods for REST API
- GET - Retrieve details of a resource
- POST - Create a new resource
- PUT - Update an existing resource
- PATCH - Update part of a resource
- DELETE - Delete a resource


## Social media Application - Resources & Methods
- Users REST API
	- Retrieve all Users
		- GET/users
	- Create a User
		- POST/users
	- Retrieve one User
		- GET/users/{id} -> /users/1
	- Delete a User
		- DELETE/users{id} -> /users/1
	- Posts REST API
		- Retrieve all posts for a User
			- GET/user/{id}/posts
		- Create a post for a User
			- POST/users/{id}/posts
		- Retrieve details of a post
			- GET/users/{id}/posts/{post_id}

### Retrieve all Users -> GET/users

#### UserDaoService.java
```java
//public User save(User user) {  
public User findOne(int id) {  
Predicate<? super User> predicate = user -> user.getId().equals(id);  
return users.stream().filter(predicate).findFirst().get();  
}
```

#### UserResource.java
```java
package com.in28minutes.rest.webservices.restfulwebservices.user;  
  
import java.util.List;  
  
import org.springframework.web.bind.annotation.GetMapping;  
import org.springframework.web.bind.annotation.PathVariable;  
import org.springframework.web.bind.annotation.RestController;  
  
@RestController  
public class UserResource {  
  
private UserDaoService service;  
  
public UserResource(UserDaoService service) {  
this.service = service;  
}  
  
// GET /users  
@GetMapping("/users")  
public List<User> retrieveAllUsers() {  
return service.findAll();  
}  
  
// GET /users  
@GetMapping("/users/{id}")  
public User retrieveUser(@PathVariable int id) {  
return service.findOne(id);  
}  
//public User save(User user) {  
  
  
}
```
>- @RestController