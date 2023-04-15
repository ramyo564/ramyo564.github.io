---
layout: single
title: "Exploring Java Bean vs POJO vs Spring Bean"
categories: Spring
tag: [Java,"record클래스","Starting Spring Framework"]
toc: true
toc_sticky: true
author_profile: false
sidebar:

---
# Starting Spring Framework(3)
/ Java Bean / POJO / Spring BEAN / 

- Java Bean : Classes adhering to 3 constraints:
	- 1: Have public default(no argument) constructors
	- 2: Allow access to their properties using getter and setter methods
	- 3: Implement java.io.Serializable
- POJO : Plain Old Java Object
	- No constraints
	- Any Java Object is a POJO
- Spring Bean : Any Java object that is managed by Spring
	- Spring uses IOC Container (Bean Factory or Application Context) to manage these onjects


