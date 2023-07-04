---

layout: single
title: " [Django] Building View "
categories: Django
tag: [Python,"[BIG][Django] Basic templates, URL's and VIews","[Django] get_object_or_404","[Django] templates - settings.py","[Django] 장고 전역변수"]
toc: true
toc_sticky: true
author_profile: false
sidebar:

---
# Basic templates, URL's and VIews (4)
{% raw %}
/ get_object_or_404 / templates - settings.py / 장고 Templates 설정
## Building the Category View

![](https://i.imgur.com/CYmELsZ.png)

>- 현재 카테고리에는 Jacket, Pants, Top 이렇게 3개의 항목이 있는데 이 부분을 연결시켜주려면 이전에 했던 작업에서 뷰를 연결시켜주면 된다.
>- Populate data 이걸 한국에서 정확히 뭐라고 옮겨서 말하는지 모르겠다 Annotation도 어노테이션 애노테이션 애너테이션 등등 제각각이다.
>- 그렇다고 영어 표기로된 것도 아님..
>- Populate data
>	- **to automatically add information to a list or table on a computer**: There are several ways to populate the database. SMART Vocabulary: related words and phrases. Environmental issues. agroecology.

### ecommerce>store>views.py
```python
from django.shortcuts import render
from .models import Category

def store(request):
    return render(request, 'store/store.html')

def categories(request):
    all_categories = Category.objects.all()
    return {'all_categories': all_categories} # key/value
```
>- 모델 카테고리의 모든 객체를 불러와서 키값과 벨류값으로 리턴해준다.
### settings.py
```python
TEMPLATES = [
    {
        'BACKEND': 'django.template.backends.django.DjangoTemplates',
        'DIRS': [],
        'APP_DIRS': True,
        'OPTIONS': {
            'context_processors': [
               'django.template.context_processors.debug',
              'django.template.context_processors.request',
                'django.contrib.auth.context_processors.auth',
                'django.contrib.messages.context_processors.messages',
                'store.views.categories',
            ],
        },
    },
]
```
>- settings.py 에서 뷰의 `categories`를 등록해준다.
>- This is a context processor that we're adding.
>- It'll allow it to be available our data on every single template or page s owe can reference this easily
>- templates 참고 : https://query.tistory.com/entry/%EC%9E%A5%EA%B3%A0-settingspy-%ED%85%9C%ED%94%8C%EB%A6%BF-%ED%8F%B4%EB%8D%94-%EC%9C%84%EC%B9%98-%EC%A7%80%EC%A0%95-%EB%B0%A9%EB%B2%95
>- 상품요약페이지든 장바구니든 모든 페이지에서 해당 기능을 사용하기위해 등록해준다고 이해하면 됨

### 장고 Templates 설정
- 장고에서 template는 MVC 패턴에서 view와 비슷한 기능을 한다.
- templates는 데이터를 사용자에게 보여주는 컴포넌트다.
- 이러한 template의 경로나 정보를 설정하는 곳이 TEMPLATES다.
- 기본 설정은 앱 디렉토리 내에 **'templates'** 디렉토리를 각각 생성해서 괸리하도록 구성되어 있으며, 이에 따라 위의 View에서 반환되는 템플릿 파일 경로는 다음과 같다
- ![](https://i.imgur.com/ukmZIvp.png)

>- 참고 : https://velog.io/@ansalstmd/Django-Template-%EA%B8%B0%EB%8A%A5

### 여러개의 templates 관리

참고: https://hwan-hobby.tistory.com/126
### ecommerce>store>templates>store>base.html
```python
<ul class="dropdown-menu" aria-labelledby="navbarDropdown">
	<li>
		<a class="dropdown-item" href="{% url 'store' %}"> All </a>
	</li>
	
	{% for category in all_categories %}
	<li>
		<a class="dropdown-item" href="">{{ category.name }}</a>
	</li>
	{% endfor %}
	
</ul>
```
>- Jinja랑 비슷하다고 생각했는데 Jinja 맞다 ㅋ
>- 템플릿 엔진 사용법은 대부분 비슷비슷하다.
>- 키 값으로 받은 all_categories 를 루프로 돌려서 나열해 출력해주면 된다.
>- ![](https://i.imgur.com/coUPqkT.png)

>- 부트스트랩 테마에서 자동으로 첫 글자가 대문자라서 상관없지만 해당 설정이 없는 경우 `{{ category.name | capfirst}}` 를 사용해서 처리해줄 수도 있다.
>	- 필요한 사항이 있으면 https://jinja.palletsprojects.com/en/3.1.x/ 여기서 확인하면 된다

## Styling the main product data grid 

- 메인화면에 그리드 형태로 상품나열 꾸미기

### ecommerce>store>views.py
```python
from django.shortcuts import render
from .models import Category,Product

def store(request):
    all_products = Product.objects.all()
    context = {'my_products':all_products}
    return render(request, 'store/store.html',context)

def categories(request):
    all_categories = Category.objects.all()
    return {'all_categories': all_categories} # key/value
```

### ecommerce>store>templates>store>store.html
```python
   <!-- All products section -->
   <div class="album py-5 bg-light">
    <div class="container">
      <div class="pb-3 h5"> All products </div>
      <hr>
      <br>
      <div class="row row-cols-1 row-cols-sm-2 row-cols-md-5 g-3">
        {% for product in my_products %}
          <div class="col">
            <div class="card shadow-sm">
              <img class="img-fluid" alt="Responsive image" src="{{product.image.url}}">
              <div class="card-body">
                <p class="card-text">
                  <a class="text-info text-decoration-none" href="{{product.get_absolute_url}}">{{product.title | capfirst}}</a>
                </p>
                <div class="d-flex justify-content-between align-items-center">
                  <h5>
                    ₩
                    {{product.price}}
                  </h5>
                </div>
              </div>
            </div>
          </div>
        {% endfor %}
      </div>
    </div>
  </div>
```
>- 뭔가 복잡하지만 처음에 카테고리를 루프로 돌린거랑 원리는 똑같다.
>- 카드형식의 테마도 부트스트랩에서 취향껏 선탠해서 조정하면 됨

![](https://i.imgur.com/3dnWUPE.png)




## Creating the singular product page

- 상품을 클릭하면 해당 상품에 대한 싱글 페이지를 만들기

### ecommerce>store>urls.py
```python
from django.urls import path
from . import views

urlpatterns = [
    # Store main page
    path('', views.store, name='store'),
    # Individual product
    path('product/<str:product_slug>/', views.product_info, name='product-info'),
]
```
>- slug 가 현재 한글로 되어 있어서 범용성을 위해 str로 변경
>- 슬러그를 통한 고유링크가 product_info 랑 연결되고 URL 매핑에 name 속성 부여 -> 'product-info'
>- 참고 : https://wikidocs.net/70741


### ecommerce>store>views.py
```python
from django.shortcuts import get_object_or_404

def product_info(request, product_slug):
    product = get_object_or_404(Product, slug=product_slug)
    context = {'product': product }
    return render(request, 'store/product-info.html', context)
```
>- get_object_or_404 는 모델 Product에서 slug가 파라미터 product_slug로 받은 값과 일치하는걸 갖고 온다.
>- 갖고 온 값을 context 의 value 값에 넣음
>- context = context 라고 해도 되고 그냥 context라고 해도 된다 (자리 순서만 맞으면 됨)


### ecommerce>store>templates>store>product-info.html
```python
<img class="img-fluid mx-auto d-block" alt="Responsive image" src="{{ product.image.url }}">
{{ product.title }}
{{ product.brand }}
{{ product.description }}
{{ product.price }}
```
>- 키값 product에서 갖고 올 때 이미지는 static 폴더에 있으니 .url 까지 써줘야한다.

![](https://i.imgur.com/VussrKo.png)


## Creating dynamic links

![](https://i.imgur.com/Qa6vOxx.png)

>- 이제 메인 페이지에서 해당 상품을 클릭하면 그 페이지로 넘어가게 설정을 해보자


### ecommerce>store>models.py
```python
class Category(models.Model):
    def get_absolute_url(self):
        return reverse("list-category", args=[self.slug])
        
class Product(models.Model):
    def get_absolute_url(self):
        return reverse("product-info", args=[self.slug])
```
>- 클래스 Category, Product 에 get_absolute_url 함수를 선언해준다.
>- urls.py 에서 path에 name으로 지정한 product-info으로 url을 연결해준다.

### ecommerce>store>templates>store>store.html
```python
<p class="card-text">
	<a class="text-info text-decoration-none" href="{{product.get_absolute_url}}">
		{{product.title | capfirst}}
	</a>
</p>
```
>- 키값 product에서 함수 get_absolute_url 를 불러온 값이 싱글페이지 주소랑 같으므로 해당 상품의 페이지로 연결된다.

## Building the list category view

![](https://i.imgur.com/j7ycK0r.png)

>- 이쪽 카테고리도 연결시켜보자

### ecommerce>store>urls.py
```python
from django.urls import path
from . import views
urlpatterns = [
    # Store main page
    path('', views.store, name='store'),
    
    # Individual product
    path('product/<str:product_slug>/', views.product_info, name='product-info'),

    # Individual category
    path('search/<str:category_slug>/', views.list_category, name='list-category'),
]
```
>- 이전과 같은 원리다.


### ecommerce>store>views.py
```python
def list_category(requst, category_slug = None):
    category = get_object_or_404(Category, slug=category_slug)
    products = Product.objects.filter(category = category)
    return render(requst, 'store/list-category.html', {'category': category, 'products':products})
```
>- 카테고리와 맞는 상품들만 객체로 옮김


### ecommerce>store>models.py
```python
class Category(models.Model):
    def get_absolute_url(self):
        return reverse("list-category", args=[self.slug])
```

### ecommerce>store>templates>store>base.html
```python
{% for category in all_categories %}
<li>
	<a class="dropdown-item" href="{{category.get_absolute_url}}">
	{{ category.name }}
	</a>
</li>
{% endfor %}
```

### ecommerce>store>templates>store>list-category.html
```python
{% extends "./base.html" %}
{% load static %}
{% block content %}
  <main>
    <div class="album py-5 bg-light">
      <div class="container">
        <div class="pb-3 h5">{{category.name | capfirst}}
          <!-- category.name -->
        </div>
        <hr>
        <br>
        <div class="row row-cols-1 row-cols-sm-2 row-cols-md-5 g-3">
          <!-- for product in products -->
          {% for product in products %}
            <div class="col">
              <div class="card shadow-sm">
                <img class="img-fluid" alt="Responsive image" src="{{product.image.url}}">
                <div class="card-body">
                  <p class="card-text">
                    <a class="text-info text-decoration-none" href="{{product.get_absolute_url}}">
                      {{product.title}}
                      <!-- product title -->
                    </a>
                  </p>
                  <div class="d-flex justify-content-between align-items-center">
                    <h5>
                      ₩
                      {{product.price}}
                      <!-- product price -->
                    </h5>
                  </div>
                </div>
              </div>
            </div>
          {% endfor %}
        </div>
      </div>
    </div>
  </main>
{% endblock %}
```
>- 필터링 된 상품들만 루프를 돌면서 보내준다.
>- ![](https://i.imgur.com/AhmWElU.png)


{% endraw %}