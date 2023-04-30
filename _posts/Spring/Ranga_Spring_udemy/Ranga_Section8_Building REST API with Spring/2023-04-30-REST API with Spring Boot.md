---

layout: single
title: " REST API with Spring Boot "
categories: Spring
tag: [Java,"[BIG] Building REST API with Spring Boot",]
toc: true
toc_sticky: true
author_profile: false
sidebar:

---
# Building REST API with Spring Boot (1)

/ REST API with Spring Boot /

## Building REST API with Spring Boot - Goals

- Why Spring Boot?
	- You can build REST API without Spring Boot 
	- What is the need for Spring Boot?
- HOW to build a great REST API?
	- Identifying Resources (/users, /users/{id}/posts)
	- Identifying Actions (GET,POST,PUT,DELETE,...)
	- Defining Request and Response structures
	- Using appropriate Response Status (200, 404, 500...)
	- Understanding REST API Best Practices
		- Thinking from the perspective of your consumer
		- Validation, Internationalization - i18n, Exception Handling, HATEOAS, Versioning, Documentation, Content Negotiation and a lot more!

## Building REST API with Spring Boot - Approach
- 1. Build 3 SImple Hello World REST API
	- Understand the magic of Spring Boot
	- Understand fundamentals of building REST API with Spring Boot
		- @RestController, @RequestMapping, @PathVariable, JSON conversion
- 2. Build a REST API for a Social Media Application
	- Design and Build a Great REST API
		- Choosing the right URI for resources(/users, /users/{id},/users/{id}/posts)
		- Choosing the right request method for actions (GET, POST, PUT, DELETE,...)
		- Designing Request and Response structures
		- Implementing Security, Validation and Exception Handling
	- Build Advanced REST API Features
		- Internationalization, HATEOAS, Versioning, Documentation, Content Negotiation, ..
- 3. Connect your REST API to a Database
	- Fundamentals of JPA and Hibernate
	- Use H2 and MySQL as database