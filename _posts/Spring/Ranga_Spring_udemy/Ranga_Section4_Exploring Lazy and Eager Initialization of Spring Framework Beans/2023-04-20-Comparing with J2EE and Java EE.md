---

layout: single
title: "Comparing with J2EE and Java EE"
categories: Spring
tag: [Java, "Comparing with J2EE and Java EE" ]
toc: true
toc_sticky: true
author_profile: false
sidebar:

---
# Comparing with J2EE and Java EE
/ J2EE / Java EE /
- Enterprise capabilities were initially built into JDK
- With time, they were separated out:
	- J2EE - Java 2 Platform Enterprise Edition
	- Java EE - Java Platform Enterprise Edition (Rebranding)
	- Jakarta EE - (Oracle gave Java EE rights to the Eclipse)
		- Important Specifications:
			- Jakarta Server Pages (JSP)
			- Jakarta Standard Tag Library(JSTL)
			- Jakarta Enterprise Beans (EJB)
			- Jakarta RESTful Web Services(JAX-RS)
			- Jarkata Bean Validation
			- Jakarta Contexts and Dependency Injection (CDI)
			- Jakarta Persistence (JPA)
		- Supported by Spring 6 and Spring boot 3
			- That's why we use jarkata, packages (instead of javax)