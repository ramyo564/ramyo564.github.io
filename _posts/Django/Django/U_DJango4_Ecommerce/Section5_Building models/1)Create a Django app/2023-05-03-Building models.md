---

layout: single
title: " [Django] Building models "
categories: Django
tag: [Python,"[BIG][Django] Building models","[Django] db_index","[Django] slug"]
toc: true
toc_sticky: true
author_profile: false
sidebar:

---
# Building models (1)

/ db_index / slug

## Create a Django app
```cmd
django-admin startapp store
```
>- 이전에 만들어 놓았던 ecommerce 폴더에서 store 앱을 생성해준다.
>- 설정에서 지금 만든 store 앱을 등록해준다.
>- ![](https://i.imgur.com/E9SqRcs.png)


## Building models
```python
from django.db import models
class Category(models.Model):
    name = models.CharField(max_length=250, db_index=True)
    slug = models.SlugField(max_length=250, unique=True)
    
    class Meta:
        verbose_name_plural = 'categories'
    def __str__(self):
        return self.name
class Product(models.Model):
    title = models.CharField(max_length=250)
    brand = models.CharField(max_length=250, default='un-branded')
    description = models.TextField(blank=True)
    slug = models.SlugField(max_length=255)
    price = models.DecimalField(max_digits=4, decimal_places=2)
    image = models.ImageField(upload_to='images/')
    class Meta:
        verbose_name_plural = 'products'
    def __str__(self):
        return self.title
```

### db_index
>- If we set `db_index` to true, this is used for query acceleration and it can be used to improve memory usage and for faster lookups in our database table.
>- So instead of reading the entire database table or our model until it finds the required attribute, it's going to stop once it's found the attribute or not, read the entire model.
>- So that's basically what this DB index is for.
>- DB에 인덱스 번호를 매긴다는 의미다.

### slug
>- A "slug" is a way of generating a valid URL, generally using data already obtained. 
>- For instance, a slug uses the title of an article to generate a URL.
>- 중복되지 않게 unique = True
>- 구글 검색 최적화에도 좋다.
>- 슬러그를 수동으로 설정하는것 보다는 함수를 통해 생성해주는게 좋다.
>	- 참고 : https://itmining.tistory.com/119

### verbose_name_plural
>- 내가 등록한 모델 이름을 장고 어드민이 알아서 복수로 만들어주지만 categorys 는 말이 안되기 때문에 이와 따로 설정해준다.

### `__str__`
>- 파이썬 내장 모듈을 통해 쉽게 출력

> 위에 기술 되어있는 내용들은 ***Building models (4)*** 포스팅에서 어떻게 구동되는지 확인할 수 있다.