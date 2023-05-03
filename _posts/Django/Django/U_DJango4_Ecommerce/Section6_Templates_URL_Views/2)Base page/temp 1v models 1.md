---

layout: single
title: " [Django] Creating a base page "
categories: Django
tag: [Python,"[BIG][Django] Basic templates, URLS's and VIews",]
toc: true
toc_sticky: true
author_profile: false
sidebar:

---
# Basic templates, URLS's and VIews (2)

/ !!! /

## Creating a base page
```python
{% load static %}
<header></header>
<body>
    {% block content %}


    {% endblock %}
</body>

```
## Creating the initial store page


