---
layout: single
title: "Thymeleaf with Spring Boot"
categories: Spring
tag: [Java,"Thymeleaf"]
toc: true
toc_sticky: true
author_profile: false
sidebar:

---
# What is Thymeleaf?
- Thymeleaf is Java templating engine
- Commonly used to generate the HTML views for web apps
- However, it is a general purpose templating engine
	- Can use Thymeleaf outside of web apps (more on this later)

>- You can create Java apps with Thymeleaf
>- No need for Spring
>- But there is a lof od synergy between the two projects

- Can be an HTML page with some Thymeleaf expressions
- Include dynamic content from Thymeleaf expressions
> Can access Java code, objects Spring beans

## Where is the Thymeleaf template porcessed?
- In a web app, Thymeleaf is processed on the server
- Results included in HTML returned to browser
![](https://i.imgur.com/RHY6PRY.png)

![](https://i.imgur.com/5q9qeq5.png)

## Development Process
1. Add Thymeleaf to Maven POM file
2. Develop Spring MVC Controller
3. Create Thymeleaf template

### Step 1: Add Thymeleaf to Maven pom file
![](https://i.imgur.com/u7xH4Zy.png)
> Based on this, Spring Boot will auto configures to use Thymeleaf templates

### Step 2: Develop Spring MVC Contoller

![](https://i.imgur.com/wA5XhEk.png)


## Where to place Thymeleaf template?
- In Spring Boot, your Thymeleaf template files go in
	- src/main/resources/templates
- For web apps, Thymeleaf templates have a .html extension

### Step 3: Create Thymeleaf template

![](https://i.imgur.com/FMDhnrV.png)


## Additional Features
- Looping and conditionals
- CSS and JavaScript integration
- Template layouts and fragments

## Using CSS with Thymleaf Templates
- You have the option of using
	- Local CSS files as part of your project
	- Referencing remote CSS files
- We'll cover both options in this video

## Development Process
1. Create CSS file
2. Reference CSS in Thymeleaf template
3. Apply CSS style

### Step 1: Create CSS file
- Spring Boot will look for static resources in the directory
	- src/main/resources/static


![](https://i.imgur.com/YjymuEM.png)


### Step 2: Reference CSS in Thymeleaf template

![](https://i.imgur.com/lxSxBy3.png)


### Step 3: Apply CSS

![](https://i.imgur.com/epQkJFX.png)


### Other search directories
- Spring Boot will search following directories for static resources:
> /src/main/resources

![](https://i.imgur.com/M1Z4klr.png)


### 3rd Party CSS Libraries - Bootstrap
- Local Installation
- Download Bootstrap file(s) and add to ***/static/css*** directory

![](https://i.imgur.com/OP9ntsC.png)
- Remote files

![](https://i.imgur.com/cro4cyf.png)
