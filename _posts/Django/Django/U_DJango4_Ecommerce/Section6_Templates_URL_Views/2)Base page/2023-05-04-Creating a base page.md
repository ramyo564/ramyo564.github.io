---

layout: single
title: " [Django] Creating a base page "
categories: Django
tag: [Python,"[BIG][Django] Basic templates, URL's and VIews","base.html"]
toc: true
toc_sticky: true
author_profile: false
sidebar:

---
# Basic templates, URL's and VIews (2)

/ Base.html /

## Creating a base page
```python
{% load static %}
<header></header>
<body>
    {% block content %}


    {% endblock %}
</body>
```
- 테마 참고 https://bootswatch.com/
- 부트스트랩, 자바스크립트,네비게이션 바등 자주 쓰는 항목들을 베이스 페이지에 넣어서 바뀌는 부분만 갈아서 끼워쓰면 된다.

## Creating the initial store page

```python
{% extends "./base.html" %}
{% load static %}
{% block content %}
 작성
{% endblock %}
```
- 네비게이션바나 자바스크립트등은 항상 똑같고 content 안에 내용만 바뀌는 경우가 많아 대부분 이렇게 사용한다.
- extends로 베이스 html을 불러오고 그 안에 내용만 갈아끼운다는 개념
>- 스프링이든 플라스크든 이 개념은 다 똑같다.