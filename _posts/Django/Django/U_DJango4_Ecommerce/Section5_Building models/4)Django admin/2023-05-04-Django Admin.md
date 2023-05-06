---

layout: single
title: " [Django] admin configuration "
categories: Django
tag: [Python,"[BIG][Django] Building models","[Django] Django Admin","[Django] Slug 한글화"]
toc: true
toc_sticky: true
author_profile: false
sidebar:

---
# Building models (4)

/ Django Admin / Slug 한글화

## Basic - Django admin
```python
from django.contrib import admin
from .models import Category, Product
# 함수기반
admin.site.register(Category)
admin.site.register(Product)
```

```cmd
python manage.py createsuperuser
```

- 파이썬 서버를 띄우고 `http://127.0.0.1:8000/admin/` 을 접속한 후 설정한 유저 네임과 비밀번호를 입력한다.
- ![](https://i.imgur.com/RaWSmEW.png)

### verbose_name_plural
![](https://i.imgur.com/lALbxrA.png)

- 장고에서는 Category를 그냥 전부 일괄적으로 Categorys 라고 생성해준다.
- verbose_name_plural 를 통해 커스텀 해주면 커스텀한 문자 내용을 어드민을 통해 확인할 수 있다.


### `__str__`

```python
class Category(models.Model):
    name = models.CharField(max_length=250, db_index=True)
    slug = models.SlugField(max_length=250, unique=True)
    class Meta:
        verbose_name_plural = 'categories'
    def __str__(self):
        return self.name
```
class Category 에서 `__str__` 모듈을 사용해서 name을 설정해줬기 때문에 Name으로 반환된다.
- ![](https://i.imgur.com/4gzv8ak.png)
- ![](https://i.imgur.com/uvMY9Tu.png)
- 이렇게 지정하지 않으면 Category(1) 이런식으로 나와서 나중에 뭐가 어떤건지 헷갈리게 된다.

## Advanced - Django - admin configuration

```python
from django.contrib import admin
from .models import Category, Product
# 함수기반
# admin.site.register(Category)
# admin.site.register(Product)
@admin.register(Category)
class CategoryAdmin(admin.ModelAdmin):
    prepopulated_fields = {'slug': ('name',)}
    
@admin.register(Product)
class ProductAdmin(admin.ModelAdmin):
    prepopulated_fields = {'slug': ('title',)}
```
>- 어드민에서 자동으로 슬러그가 자동으로 등록되게 만들어준다.
>- 이렇게 해놓으면 슬러그를 따로 설정할 필요없이 설정해 놓은 변수대로 자동으로 만든다.
>- ![](https://i.imgur.com/TtvDGL2.png)

## Slug 한글화

![](https://i.imgur.com/KeUqSRx.png)
- 샘플로 쇼핑몰을 만들려고 보는데 한글은 Slug가 지원이 기본으로 안된다.
	- `slugify` 를 이용하면 된다.
```python
from django.db import models
from django.utils.text import slugify
class Category(models.Model):
    name = models.CharField(max_length=250, db_index=True)
    slug = models.SlugField(max_length=250, unique=True, allow_unicode=True)
    
    class Meta:
        verbose_name_plural = 'categories'
    def __str__(self):
        return self.name
```
>- `slugify` 를 부르고 slug 파라미터에 `allow_unicode=True` 로 설정해주면 된다.
>- ![](https://i.imgur.com/r8NKOIy.png)


## Slug 한글 오류
- slug를 한글로 사용하면 url에서 사용할 떄 오류가 난다.
- ![](https://i.imgur.com/vnrQs3G.png)
```python
from django.urls import path
from . import views

urlpatterns = [
    # Store main page
    path('', views.store, name='store'),
    # Individual product
    path('product/<slug:product_slug>/', views.product_info, name='product-info'),
	-> slug 대신에 str로 변경
]
```
>- slug를 그냥 str로 변경하면 해결  -> `<str:product_slug>` 
>- re_path나 StringConverter를 사용해서 해결할 수도 있지만 이게 제일 간단하고 편하다.
>	- 어차피 위에 있는 방법도 원리는 비슷비슷하다.
>- ![](https://i.imgur.com/C7UkOfI.png)
>	- URL에 한글이 있어도 잘 갖고 온다.

>- 참고 : https://kgu0724.tistory.com/99