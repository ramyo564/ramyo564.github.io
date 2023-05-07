---

layout: single
title: " [Django] Cart functionality - Initial build up "
categories: Django
tag: [Python,"[BIG][Django] Cart functionality",]
toc: true
toc_sticky: true
author_profile: false
sidebar:

---
# Cart (2)
{% raw %}
/ 장바구니 만들기 /

## Cart app 만들기

```Terminal
django-admin startapp cart
```
>- 터미널에서 cart app을 만든후 settings.py 에서 cart를 INSTALLED_APPS에 등록해준다.
>- cart 폴더에 urls.py 를 만들어준다.
>- 그 후 메인 urls.py에 cart를 등록해준다.
>- ![](https://i.imgur.com/On1p8uH.png)

## urls.py 

```python
from django.urls import path
from . import views

urlpatterns = [
    # Cart main page
    path('', views.cart_summary, name='cart-summary'),
    path('add/', views.cart_add, name='cart-add'),
    path('delete/', views.cart_delete, name='cart-delete'),
    path('update/', views.cart_update, name='cart-update'),
]
```
>- CRUD 경로 설정

### base.html 카트 경로 설정
```html
  
<a type="button" role="button" href="{% url 'cart-summary' %}" class="btn btn-outline-secondary">
	<i class="fa fa-shopping-cart" aria-hidden="true"> </i>
</a>
```
>- 카트 아이콘을 클릭하면 urlpatterns 를 통해 views.cart_summary로 연결


### views.py
```python
def cart_summary(request):
    return render(request, 'cart/cart-summary.html')
```
>- cart-summary.html 렌더링


{% endraw %}
