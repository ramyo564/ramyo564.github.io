---
layout: single
title: "Spring Data REST Configuration, Pagination and Sorting"
categories: Spring
tag: [Java,REST,"Data REST",HATEOAS,"@RepositoryRestResource","REST Pagination Soritng"]
toc: true
toc_sticky: true
author_profile: false
sidebar:

---

# REST Endpoints
REST CRUD APIs(11)
- By default, Spring Data REST will create endpoints based on entity type
- Simple pluralized form
	- First character of Entity type is lowercase
	- Then just adds an "s" to the entity

```java
public interface EmployeeRepository extends JpaRepository<Employee, Integer>{
 // the Employee -> /employees <- "s"
}
```

## Pluralized Form
- Spring Data REST pluralized form is VERY simple
	- Just adds an "s" to the entity
- The English language is VERY complex!
	- Spring Data REST does NOT handle

| Singular | Plural |
| -------- | ------ |
| Goose    | Geese  |
| person   | People |
| Sylabus  | Syllabi       |

## Problem
- Spring Data REST does not handle complex pluralized forms
	- In this case, you need to specify plural name
- What if we want to expose a different resource name?
	- Instead of / employees .. use/members

## Solution
- Specify plural name / path with an annotation
```java
@RepositoryRestResource(path="members")
public interface EmployeeRepository extends JpaRepository<Employee, Integer> {
 -> http://localhost:8080/members 
}
```

## Pagination
- By default, Spring Data REST will return the first 20 elements
	- Page size = 20
- You can navigate to the different pages of data using query param
	- http://localhost:8080/employees?page=0 <- Pages are zero-based
	- http://localhost:8080/employees?page=1

## Spring Data REST Configuration
- Following properties available: application.properties

| Name                               | Description                                   |
| ---------------------------------- | --------------------------------------------- |
| spring.data.rest.base-path         | Base path used to expose repository resources |
| spring.data.rest.default-page-size | Default size of pages                         |
| spring.data.rest.max-page-size     | Maximum size of pages                         |
| ....                               | ...                                           |


## Sample Configuration

```java
File: application.properties
spring.data.rest.base-path=/magic-api <- http://localhost:8080/magic-api/employees
spring.data.rest.default-page-size=50 <- Returns 50 elements per page
```

## Sorting
- You can sort by the property names of your entity
	- In our Employee example, we have: ***firstName***, ***lastName*** and ***email***
- Sort by last name (ascending is default) http://localhost:8080/employees?sort=lastName
- Sort by first name, descending http://localhost:8080/employees?sort=firstName,desc
- Sort by last name, then first name, ascending http://localhost:8080/employees?sort=lastname.firstName,asc

## Configuration, Pagination, Soritng

![](https://i.imgur.com/LNoW1hy.png)


![](https://i.imgur.com/Y1T6sQw.png)


![](https://i.imgur.com/lvTtQGR.png)

### Meta-data-about the page

![](https://i.imgur.com/LRi1mYz.png)


![](https://i.imgur.com/9EgsQfF.png)

![](https://i.imgur.com/7RBaW0d.png)
> Remember page numbers are zero-based

### Sort by last name
> - http://localhost:8080/magic-api/employees?sort=lastName
> - http://localhost:8080/magic-api/employees?sort=lastName,descending
> 

