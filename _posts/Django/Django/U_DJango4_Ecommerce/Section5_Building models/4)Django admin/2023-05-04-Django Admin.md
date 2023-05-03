---

layout: single
title: " [Django] admin configuration "
categories: Django
tag: [Python,"[BIG][Django] Building models",]
toc: true
toc_sticky: true
author_profile: false
sidebar:

---
# Building models (4)

/ !!! /

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
