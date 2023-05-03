---

layout: single
title: " [Spring] Spring -Framework,Modules and Projects "
categories: Spring
tag: [Java,"Spring -Framework,Modules and Projects",]
toc: true
toc_sticky: true
author_profile: false
sidebar:

---
# Spring -Framework,Modules and Projects
/ IOC /

## Spring core : IOC Container, Dependency Injection, Auto Wiring, ...
- ![](https://i.imgur.com/yjrJYpN.png)
- These are the fundamental building blocks to:
	- Building web applications
	- Creating REST API
	- Implementing authentication and autorization
	- Talking to a database
	- Integrating with other systems
	- Writing great unit tests
- Let's now get a Spring Big Picture:
	- Spring Framework
	- Spring Modules
	- Spring Projects

## Spring Framework contains multiple Spring Modules:
- ![](https://i.imgur.com/x63eOmb.png)
	- Fundamental Features : Core (IOC Container, Dependency Injection, Auto Wiring, ...)
	- Web: Spring MVC etc (Web applications, REST API)
	- Web Reactive : Spring WebFlux etc
	- Data Access : JDBC, JPA etc
	- Integration : JMS etc
	- Testing : Mock Objects, Spring MVC Test etc
- No Dumb Question : Why is Spring Framework divided into Modules?
	- Each application can choose modules they want to make use of 
	- They do not need to make use of everything in Spring framework!

## Spring Projects
- ![](https://i.imgur.com/s1cQcaw.png)
- Application architectures evolve continuously
	- Web > REST API > Microservices > Colud > ...
- Spring evolves through Spring Projects:
	- First Project : Spring Framework
	- Spring Security : Secure your web application or REST API or microservice
	- Spring Data : Integrate the same way with different types of databases : NoSQL and Relational
	- Spring Integration : Address Challenges with integration with other applications
	- Spring Boot : Popular framework to build microservices
	- Spring Cloud : Build cloud native applications
- Hierachy : Spring Projects > Spring Framework > Spring Modules
- Why is Spring Eco system popular?
	- Loose Coupling : Spring manages creation and wiring of beans and dependencies
		- Make it easy to build loosely coupled applications
		- Make writing unit tests easy! (Spring Unit Testing)
	- Reduced Boilerplate code : Focus on Business Logic
		- Example : No need for exception handling in each method!
			- All checked exceptions are converted to Runtime or Unchecked Exceptions
		- Archtiectural Flexibility : Spring Modules and Projects
			- You can pick and choose which ones to use (You DON'T need to use all of them!)
		- Evolution with Time : Microservices and Cloud
			- Spring Boot, Spring Cloud etc!