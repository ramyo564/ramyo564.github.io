---

layout: single
title: " Implementing DELETE Method to delete a User Resource "
categories: Spring
tag: [Java,"[BIG] Building REST API with Spring Boot",]
toc: true
toc_sticky: true
author_profile: false
sidebar:

---
# Building REST API with Spring Boot (6)

/ !!!!!! /

## Implementing DELETE Method to delete a User Resource

### UserResource.java
```java
// Delete /users  
@DeleteMapping("/users/{id}")  
public void deleteUser(@PathVariable int id) {  
	service.deleteById(id);  
}
```

### UserDaoService.java
```java
public void deleteById(int id) {  
	Predicate<? super User> predicate = user -> user.getId().equals(id);  
	users.removeIf(predicate);  
}
```