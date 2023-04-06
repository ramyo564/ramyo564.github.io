---
layout: single
title: "Spring Data REST"
categories: Spring
tag: [Java,REST,"Data REST",HATEOAS]
toc: true
toc_sticky: true
author_profile: false
sidebar:

---
# Spring Data REST
REST CRUD APIs(10)

## Spring Data JPA
- Earlier, We saw the magic of Spring Data JPA
- This helped to eliminate boilerplate code

## Can this apply to REST APIs?

### The Problem
- We saw how to create a REST API for ***Employee***
- Need to create REST API for another entity?
	- Customer, Student, Product, Book..
- Do we have to repeat all of the same code again?

### Wish
>- Create a REST API for me
>- Use my existing JpaRepository (entity,primary key)
>- Give me all of the basic REST API CRUD features for free

### Spring Data REST - Solution
- Spring Data REST is the solution
- Leverages your existing JpaRepository
- Spring will give you a REST CRUD implementation for FREE
	- Helps to minimize boiler-plate REST code
	- No new coding required

## REST API
- Spring Data REST will expose these endpoints for free

| HTTP Method |                         | CRUD Action                 |
| ----------- | ----------------------- | --------------------------- |
| POST        | /employees              | Create a new employee       |
| GET         | /employees              | Read a list of employees    |
| GET         | /employees/{employeeId} | Read a single employee      |
| PUT         | /employees/{employeeId} | Update an existing employee |
| DELETE      | /employees/{employeeId} | Delete an existing employee |


>Get these REST endpoints for free

## Spring Data REST - How Does It Work?
- Spring Data REST will scan your project for ***JpaRepository***
- Expose REST APIs for each entity type for your ***JpaRepository***


```java
public interface EmployeeRepository extends JpaRepository<Employee, Integer> {

}
```

## REST Endpoints
- By default, Spring Data REST will create endpoints based on entity type
- Simple pluralized form
	- First character of Entity type is lowercase
	- Then just adds an "s" to the entity

```java
public interface EmployeeRepository extends JpaRepository<Employee, Integer>{

}
```

## Development Process
1. Add spring Data REST to your Mavern POM file
>- That's it
>- Absolutely NO CODING required

### Step 1: Add Spring Data REST to POM file
![](https://i.imgur.com/oUMFSl4.png)

```java
<dependency>
	<groupId>org.springframework.boot</gropId>
	<artifactId>spring-boot-starter-data-rest</artifactId>
</dependency>
```
>Spring Data REST will scan for JPARepository

![](https://i.imgur.com/naaHTHk.png)

![](https://i.imgur.com/s5MDtlx.png)


## In A Nutshell

For Spring Data REST, you only need 3 items
1. Your entity : Employee
2. JpaRepository : EmployeeRepository extends JpaRepository (We already have these two)
3. Maven POM dependency for: spring-boot-starter-data-rest (Only item that is new)

## Application Architecture

![](https://i.imgur.com/eJg7IR3.png)

## Minimized Boilerplate Code

- Before Spring Data RREST
	- RESST controller
	- employee service
	- service implementation
> about 3 files and a hundred puls lines of code

- After Spring Data REST
>- NO coding required 
>- Just give a Maven POM entry
>- Spring Data REST will scan for those repositories
>- Automatically expose those endpoints for free

## HATEOAS
- Spring Data REST endpoints are HATEOAS compliant
	- HATEOAS: Hypermedia as the Engine of Application State
- Hypermedia-driven sites provide information to access REST interfaces
	- Think of it as meta-data for REST data
- Spring Data REST response using HATEOAS
- For example REST response from : GET / employees / 3
- Then it'll return the JSON data as a response
- For a collection, meta-data includes page size, total elements, pages etc
- For example REST response from: GET / employees
- 133pic 07..IFIW

## Advanced Features
- Spring Data REST advanced features
	- Pagination, sorting and searching
	- Extending and adding custom queries with JPQL
	- Query Domain Specific Language(Query DSL)

