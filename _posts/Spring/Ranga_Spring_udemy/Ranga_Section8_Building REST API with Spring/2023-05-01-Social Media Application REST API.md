---

layout: single
title: " Social Media Application REST API "
categories: Spring
tag: [Java,"[BIG] Building REST API with Spring Boot","ResponseEntity","ServletUriComponentsBuilder","buildAndExpand","toUri"]
toc: true
toc_sticky: true
author_profile: false
sidebar:

---
# Building REST API with Spring Boot (4)

/ ResponseEntity / ServletUriComponentsBuilder / buildAndExpand / toUri

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

### Retrieve all Users  & findOne
-> GET/users
-> GET/users/{id} -> /users/1

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
>- @RestController 는 @Controller에 @ResponseBody가 추가된 것
>- RestController의 주용도는 Json 형태로 객체 데이터를 반환하는 것
>	- 참고 : https://mangkyu.tistory.com/49
>- id 값은 숫자니 int로 받고 @PathVariable 를 통해 받아올 때 {} 사용하면된다.
>- User 클라스에서 findOne 선언
>	- 유저아이디가 일치하는게 있으면 리턴
>- ![](https://i.imgur.com/GWDXVhW.png)
>- ![](https://i.imgur.com/nVtzsnN.png)

### Create a User
-> POST/users
#### UserResource.java
```java
//POST /users  
@PostMapping("/users")  
public void createUser(@RequestBody User user) {  
service.save(user);  
}
```
>- @PostMapping 와 @RequestBody 를 이용해 데이터를 받는다.
>- 아이디 값은 자동 카운팅으로 변경 했으니 이름과 생년월일만 post 맵에 맞게 적으면 알아서 데이터가 들어간다.

#### UserDaoService.java
```java
private static int usersCount = 0;

public User save(User user) {
		user.setId(++usersCount);
		users.add(user);
		return user;
	}

```
>- 이전에 저장해둔 임의 값의 아이디도 변경해준다.
>	- `users.add(new User(1,"Adam",LocalDate.now().minusYears(30)));`
>	- -> `users.add(new User(++usersCount,"Adam",LocalDate.now().minusYears(30)));`

### POSTMAN 으로 테스트하기
- ![](https://i.imgur.com/UhsCkO5.png)
- ![](https://i.imgur.com/dmHuHbe.png)
- 자동으로 아이디 값도 잘 반영되는지 확인
- 하지만 아직 응답이 200으로 반영됨
	- 뭔가 생성 되었을 때는 201로 반영되야함

#### UserResource.java
```java
@PostMapping("/users")  
public ResponseEntity<User> createUser(@RequestBody User user) {  
  
User savedUser = service.save(user);  
  
URI location = ServletUriComponentsBuilder.fromCurrentRequest()  
				.path("/{id}")  
				.buildAndExpand(savedUser.getId())  
				.toUri();  
  
return ResponseEntity.created(location).build();  
}
```
>- ResponseEntity 를 통하면 201이 정상적으로 반환된다 .
>- ServletUriComponentsBuilder.fromCurrentRequest() -> 사용자가 요청한 URI를 갖고 온다.
>- path에 변수명 입력
>- buildAndExpand에 갖고 올 정보를 입력하면 {id}에 그 값이 추가된다.
>- toUri() 는 uri 생성이다.
>- **build()메서드**는 필수 멤버변수의 null체크를 하고 지금까지 set된 builder를 바탕으로 A클래스의 생성자를 호출하고 인스턴스를 리턴한다.
>	- `2<201 CREATED Created,[Location:"http://localhost:8080/users/4"]>`
>	- ![](https://i.imgur.com/M3G00C0.png)


- 참고 : https://pooney.tistory.com/65 , https://devjjsjjj.tistory.com/entry/%EB%B9%8C%EB%8D%94-%ED%8C%A8%ED%84%B4-Builder-Pattern-Builder
