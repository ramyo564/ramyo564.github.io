---

layout: single
title: " [Spring] AutoConfiguration Magic Data JPA "
categories: Spring
tag: [Java,"[BIG][Spring] Java Web Application with Spring and Hibernate",]
toc: true
toc_sticky: true
author_profile: false
sidebar:

---
# Java Web Application with Spring and Hibernate (12)

/ JPA /

![](https://i.imgur.com/a8j0ZGI.png)
- We added Data JPA and H2 dependencies:
	- Spring Boot Auto Configuration does some magic:
		- Initialize JPA and Spring Data JPA frameworks
		- Launch an in memory database(H2)
		- Setup connection from App to in-memory database
		- Launch a few scripts at startup (example: data.sql)
- Remember - H2 is in memory database
	- Does NOT persist data
	- Great for learning
	- BUT NOT so great for production
	- Let's see how to use MySQL next!

