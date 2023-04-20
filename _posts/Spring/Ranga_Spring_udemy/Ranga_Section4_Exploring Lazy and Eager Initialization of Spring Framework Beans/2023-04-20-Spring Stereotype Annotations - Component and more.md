---

layout: single
title: " Spring Stereotype Annotations - Component and more "
categories: Spring
tag: [Java,"Spring Stereotype Annotations - Component and more","@Service","@Controller","@Repository"]
toc: true
toc_sticky: true
author_profile: false
sidebar:

---
# Spring Stereotype Annotations - Component and more
/ @Component / @Repository / @Service / @Controller / 

- @Component - Generic annotation applicable for any class
	- Base for all Spring Stereotype Annotations
	- Specializations of @Component:
		- @Service - Indicates that an annotated class has business logic
		- @Controller - Indicates that an annotated class is a "Controller" (e.g. a web controller)
			- Used to define controllers in your web applications and REST API
		- @Repository - Indicates that an annotated class is used to retrieve and/or manipulate data in a database
- What should you use?
	- (RECOMMENDATION) Use the most specific annotation possible
	- Why?
		- By using a specific annotation, you are giving more information to the framework about your intentions
		- You can use AOP at later point to add additional behavior
			- Example : For @Repository, Spring automatically wires in JDBC Exception translation features