---
layout: single
title: "How can I list all beans managed by Spring Framework?"
categories: Spring
tag: [Java,"Starting Spring Framework",]
toc: true
toc_sticky: true
author_profile: false
sidebar:

---
# Starting Spring Framework(4)
/ AutoWiring //

## How can I list all beans managed by Spring Framework?
- ![](https://i.imgur.com/IQzOS1h.png)
>- 위와 같이 스트림을 이용해서 관리되는 Bean 목록을 가져와 볼 수 있다.


## What if multiple matching beans are available?
- @Primary , @Qualifier 를 사용한다.
- 태그 "@Primary","@Qualifier" 에 자세히 정리해둠
- 다시정리
