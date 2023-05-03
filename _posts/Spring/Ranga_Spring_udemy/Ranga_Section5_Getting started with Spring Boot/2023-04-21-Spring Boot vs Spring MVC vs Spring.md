---

layout: single
title: " [Spring] Spring Boot vs Spring MVC vs Spring "
categories: Spring
tag: [Java,"Spring Boot vs Spring MVC vs Spring"]
toc: true
toc_sticky: true
author_profile: false
sidebar:

---
# Spring Boot vs Spring MVC vs Spring
/ Spring Boot / Spring MVC / Spring

- Spring Boot vs Spring MVC vs Spring : What's in it?
	- ***Spring Framework*** : Dependency Injection
		- @Component, @Autowired, Component Scan etc..
		- Just Dependency Injection is NOT sufficient (You need other frameworks to build apps)
			- If you want to talk to a database, you need hibernate or GPA
			- Spring Modules and Spring Projects: Extend Spring Eco System
				- Provide good integration with other frameworks(Hibernate/JPA, JUnit & Mockito for Unit Testing)
	- ***Spring MVC*** (Spring Module) : Simplify building web apps and REST API
		- Building web applications with Struts was very complex
		- @Controller, @RestController, @RequestMapping("/courses")
			- They make easier to build web aaplications than Struts
	- ***Spring Boot*** (Spring Project) : Build ***PRODUCTION-READY*** apps ***QUICKLY***
		- ***Starter Projects*** - Make it easy to build variety of applications
		- ***Auto configuration*** - Eliminate configuration to setup Spring, Spring MVC and other frameworks!
		- Enable non functional requirements (NFRs):
			- Actuator : Enables Advanced Monitoring of applications
			- Embedded Server : No need for separate application servers!
			- Logging and Error Handling
			- Profiles and Configureation Properties
