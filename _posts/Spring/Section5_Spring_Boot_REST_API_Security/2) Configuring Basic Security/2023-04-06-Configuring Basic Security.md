---
layout: single
title: "Configuring Basic Security"
categories: Spring
tag: [Java,"Configuring Basic Security","@Configuration","InMemoryUserDetailsManager",CSRF]
toc: true
toc_sticky: true
author_profile: false
sidebar:

---
# Our Users
REST API security(2)

| User ID | Password | Roles                    |
| ------- | -------- | ------------------------ |
| john    | test123  | EMPLOYEE                 |
| mary    | test123  | EMPLOYEE, MANAGER        |
| susan   | test123  | EMPLOYEE, MANAGER, ADMIN |

## Development Process
1. Create Spring Security Configuration(@Configuration)
2. Add users, passwords and roles

### Step 1: Create Spring Security Configuration
```java
File:DemoSecurityConfig.java
import org.springframework.context.annotation.Configuration;

@Configuration
public class DemoSecurityConfig {

	// add our security configurations here ...
}
```

### Spring Security Password Storage
- In Spring, passwords are stored using a specific format
>{id}encodedPassword

| ID     | Description             |
| ------ | ----------------------- |
| noop   | Plain text passwords    |
| bcrypt | BCrypt password hashing |
| ...       |                 ...        |

### Password Example

![](https://i.imgur.com/0E65Ank.png)

### Step 2: Add users, passwords and roles

- File: DemoSecurityConfig.java

![](https://i.imgur.com/Yovda3M.png)
> - Since we defined our users here...
> - Spring Boot will NOT use the user/pass from the application.properties file

![](https://i.imgur.com/8p6izGL.png)

## Restric Access Based on Roles

### Example

| HTTP Method | Endpoint                    | CRUD Action     | Role     |
| ----------- | --------------------------- | --------------- | -------- |
| GET         | /api/employees              | Read all        | EMPLOYEE |
| GET         | /api/employees/{employeeId} | Read single     | EMPLOYEE |
| POST        | /api/employees              | Create          | MANAGER  |
| PUT         | /api/employees              | Update          | MANAGER  |
| DELETE      | /api/employees/{employeeId} | Delete employee | ADMIN         |

### Restricting Access to Roles
- General Syntax

![](https://i.imgur.com/bZw0XsF.png)

![](https://i.imgur.com/eS728xc.png)

![](https://i.imgur.com/GVkcVQQ.png)

### Authorize Requests for EMPLOYEE role

![](https://i.imgur.com/gKKMDVw.png)

### Authorize Requests for MANAGER role

![](https://i.imgur.com/w9nCbci.png)


### Authorize Requests for ADMIN role

![](https://i.imgur.com/olM1el1.png)


## Pull it Together

![](https://i.imgur.com/Onp09sO.png)

## Cross-Site Request Forgery(CSRF)
- Spring Security can protect against CSRF attacks
- Embed additional authentication data/token into all HTML forms
- On subsequent requests, web app will verify token bofore processing
- Primary use case is traditional web applications (HTML forms etc..)

## When to use CSRF Protection?
- The Spring Security team recommends
	- Use CSRF protection for any normal browser web requests
	- Traditional web apps with HTML forms to add/modify data
- If you are building a REST API for non-browser clients
	- you may want to disable CSRF protection
- In general, not required for stateless REST APIs
	- That use POST,PUT,DELETE and/or PATCH

## Pull it Together

![](https://i.imgur.com/GobMqQ9.png)


![](https://i.imgur.com/Xjx4aKF.png)

- Role : ADMIN
- Test user : susan / test123
- pass: Get all employees
- pass: Get single employee
- pass: Add employee
- pass: Update employee
- pass: Delete employee