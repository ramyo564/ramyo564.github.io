---

layout: single
title: " Embedded Servers "
categories: Spring
tag: [Java,"Embedded Servers",]
toc: true
toc_sticky: true
author_profile: false
sidebar:

---
# Embedded Servers
/ Embedded Servers /

![](https://i.imgur.com/wni1yrq.png)

- How do you deploy your application?
	- Step 1 : Install Java
	- Step 2 : Install Web / Application Server
		- Tomcat/WebSphere/WebLogic etc
	- Step 3 : Deploy the application WAR (Web ARchive)
		- This it the OLD WAR Approach
		- Complex to setup!
- Embedded Server - Simpler alternative
	- Step 1 : Install Java
	- Step 2 : Run JAR file
	- Make JAR not WAR (Credit: Josh Long!)
	- Embedded Server Examples:
		- spring-boot-starter-tomcat
		- spring-boot-starter-jetty
		- spring-boot-starter-undertow

>- 스프링을 쓰면 톰캣이든 뭐든 알아서 해준다.