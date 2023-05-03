---

layout: single
title: " API response -> JSON? XML? -> Content Negotiation "
categories: Spring
tag: [Java,"[BIG] Advanced REST API Features","Content Negotiation","API -> json,xml, ...","api 응답 포맷 형식 변경"]
toc: true
toc_sticky: true
author_profile: false
sidebar:

---
# Advanced REST API Features (2)

/ Content Negotiation / api 응답 포맷 형식 변경

## Content Negotiation
- Same Resource - Same URI
	- HOWEVER Different Representations are possible
		- Example: Different Content Type - XML or JSON or ...
		- Example: Different Language - English or Dutch or...
- How can a sonsumer tell the REST API provider what they want?
	- Content Negotiation
	- Example: Accept header (MIME types - application/xml, application/json, ...)
	- Example: Accept-Language header (en, nl, fr, ...)

## Accept header

### pom.xml
```java
<dependency>
	<groupId>com.fasterxml.jackson.dataformat</groupId>
	<artifactId>jackson-dataformat-xml</artifactId>
</dependency>
```
>- ![](https://i.imgur.com/dqg368J.png)
>-  Dependency 추가 후 사진과 같이 Headders 부분에 Accept 를 변경하여 json 형식이 아닌 xml로 응답 받을 수 있다.


