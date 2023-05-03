---

layout: single
title: " REST API Documentation (Open API, Swagger) "
categories: Spring
tag: [Java,"[BIG] Advanced REST API Features","Open API","Swagger"]
toc: true
toc_sticky: true
author_profile: false
sidebar:

---
# Advanced REST API Features (1)

/ Swagger / Open API

## REST API Documentation
- Your REST API consumners need to understand your REST API:
	- Resources
	- Actions
	- Request/Response Structure(Constraints/Validations)
- Challenges:
	- Accuracy : How do you ensure that your documentation is up to date and correct?
	- Consistency : You might have 100s of REST API in an enterprise.
		- How do you ensure consistency?
- Options:
	- 1: Manually Maintain Documentaion
		- Additional effort to keep it in sync with code
	- 2: Generate from code

## REST API Documentation - Swagger and Open API
- Quick overview:
	- 2011: Swagger specification and Swagger Tools were introduced
	- 2016: Open API Specification created based on Swagger Spec.
		- Swagger Tools (ex: Swagger UI) continue to exist
	- Open API Specification: Standard, language-agnostic interface
		- Discover and understand REST API
		- Earlier called Swagger Specification
	- Swagger UI: Visualize and interact with your REST API

>`springdoc-openapi` java library helps to automate the generation of API documentation for spring boot projects. 

```java
   <dependency>
      <groupId>org.springdoc</groupId>
      <artifactId>springdoc-openapi-starter-webmvc-ui</artifactId>
      <version>2.1.0</version>
   </dependency>

```

https://springdoc.org/v2/
https://springdoc.org/#Introduction