---
layout: single
title: " [Django] Configure URL files and store view "
categories: Django
tags:
  - Python
  - Basic
  - VIews
toc: true
toc_sticky: true
author_profile: false
sidebar:
---
# Basic templates, URL's and VIews (1)

/ urls.py / views.py /

## Configure URL files and store view


>- store 폴더에 templates 폴더를 하나 생성해주고 거기에 다시 store 폴더를 생성해준다.
>- 테스트용으로 store.html 파일 생성
>- ![](https://i.imgur.com/XE73Hvo.png)


>-![](https://i.imgur.com/9sgCZYA.png)

>- store 폴더에 views.py 파일과 urls.py를 만들어준다.

### views.py

{% raw %}
```python
from django.shortcuts import render

def store(request):
    return render(request, 'store/store.html')
```
{% endraw %}
>- 뷰는 요청을 받아 응답을 반환해주는 호출 가능한 객체이다. 
>- Django에서는 뷰를 함수 형태로 작성할 수도 있고 클래스 형태로 작성할 수도 있다.
>- 참고 https://wikidocs.net/9649

## url 정의
{% raw %}
```python
urlpatterns = [
    url(정규식, 뷰, kwargs=None, name=None, prefix=''),
]
```
{% endraw %}
-   정규식: URL을 정규식으로 표현
-   뷰: URL 매칭이 되면 불러올 뷰 (CBV 또는 FBV)
-   kwargs: 정규식 인자에서 추출한 파라미터 외에 추가적인 인자를 파이썬 사전 타입의 키워드 인자로 뷰 함수에 전달 가능
-   name: URL 별로 별명을 두어 템플릿 파일에서 사용
-   prefix: 뷰 함수에 대한 접두사 문자열

### urls.py
{% raw %}
```python
from django.urls import path
from . import views

urlpatterns = [
    # Store main page
    path('', views.store, name='store'),
]
```
{% endraw %}
>- list 형식이라 path 끝에, 를 꼭 붙여주는게 좋다.


### ecommerce>ecommerce>urls.py
{% raw %}
```python
from django.contrib import admin
from django.urls import path, include
from django.conf import settings
from django.conf.urls.static import static

urlpatterns = [
    path('admin/', admin.site.urls),
        # Store app
    path('', include('store.urls')),
]

urlpatterns += static(settings.MEDIA_URL, document_root = settings.MEDIA_ROOT)
```
{% endraw %}

>- 메인 urls.py 에 include 함수를 호출해서 store.urls 와 연결시켜준다.

>- ![](https://i.imgur.com/LxAkJX1.png)


