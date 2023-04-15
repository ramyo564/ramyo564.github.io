---
layout: single
title: "Exploring Spring IOC Container"
categories: Spring
tag: [Java,"Bean Factory","Application Context","Starting Spring Framework"]
toc: true
toc_sticky: true
author_profile: false
sidebar:

---
# Starting Spring Framework (2)
/ Application Context / Bean Factory /

1. Spring Container vs Spring Context vs IOC container vs Application Context

## What is Spring container?

![](https://i.imgur.com/ePdLMID.png)

- Spring Container : Manages Spring beans & their lifecycle
- 1: Bean Factory : Basic Spring Container
- 2: Application Context : Advanced Spring Container with enterprise-specific features
	- Easy to use in web applications
	- Easy internationalization
	- Easy integration with Spring AOP
- Which one to use? : Most enterprise applications use Application Context
	- Recommended for web applications, web services - REST API and microservices

### Spring Container vs Spring Context vs IOC container vs Application Context 
- They are referring to the same thing.
- They are referring to the thing which takes your classes and input and creates a running system.
- And when we are talking about spring containers or spring IOC containers, there are two popular types of IOC containers.
