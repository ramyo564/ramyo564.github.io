---
layout: single
title: "[Spring] User Accounts Stored in Database"
categories: Spring
tag: [Java,"User Accounts Stored in Database",]
toc: true
toc_sticky: true
author_profile: false
sidebar:

---
# Database Access
REST API security(3)
- So far, our user accounts were hard coded in Java source code
- We want to add database access <- ADVANCED

## Recall Our User Roles


| User ID | Password | Roles    |
| ------- | -------- | -------- |
| john    | test123  | EMPLOYEE |
| marry   | test123  | EMPLOYEE,MANAGER         |

## Database Support in Spring Security
- Spring Security can read user account info from database
- By default, you have to follow Spring Security's predefined table schemes
- Follow Spring Security's predefined table schemas
- That'll give us all the functionality and all of the code for hooking into the actual database and this is all given to us out of the box

## Customize Database Access with Spring Security
- Can also customize the table schemes
- Useful if you have custom tables specific to your project / custom
- You will be responsible for developing the code to access the data
	- JDBC, JPA / Hibernate etc

## Development Process
1. Develop SQL Script to set up database tables
2. Add database support to Maven POM file
3. Create JDBC properties file
4. Update Spring Security Configuration to use JDBC

### Default Spring Security Database Schema

![](https://i.imgur.com/Nho9fuy.png)


### Step 1: Develop SQL Script to setup database tables

![](https://i.imgur.com/AU5kN0f.png)

![](https://i.imgur.com/ycjsOoD.png)

![](https://i.imgur.com/zjdEe32.png)

![](https://i.imgur.com/sYFDZtK.png)




### Step 2: Add Database Support to Maven POM file

```java
<!-- MySQL -->
<dependency>
	<groupId>com.mysql</groupId>
	<artifactId>mysql-connector-j</artifactId>
	<scope>runtime</scope>
</dependency>
```
>JDBC Driver


### Step 3: Create JDBC Properties File

```java
File: application.properties

# JDBC connection properties

spring.database.url=jdbc:mysql://localhost:3306/employee_directory
spring.database.username=springstudent
spring.database.password=springstudent
```

### Step 4: Update Spring Security to use JDBC

![](https://i.imgur.com/TrKtgKz.png)
