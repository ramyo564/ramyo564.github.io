---

layout: single
title: " [Django] !!! "
categories: Django
tag: [Python,"[Django] 상품등록",]
toc: true
toc_sticky: true
author_profile: false
sidebar:

---
# 상품 모델 짜기
{% raw %}

## Products 모델 만들기
### models.py
```python
from django.db import models
from category.models import Category
from django.utils.text import slugify

# Create your models here.

class Product(models.Model):
    product_name    = models.CharField(max_length=200, unique=True)
    slug            = models.SlugField(max_length=200, unique=True, allow_unicode=True)
    description     = models.TextField(max_length=500, blank=True)
    price           = models.IntegerField()
    images          = models.ImageField(upload_to='photos/products')
    stock           = models.IntegerField()
    is_available    = models.BooleanField(default=True)
    category        = models.ForeignKey(Category, on_delete=models.CASCADE)
    created_date    = models.DateTimeField(auto_now_add=True)
    modified_date   = models.DateTimeField(auto_now=True)

    def __str__(self):
        return self.product_name

    def save(self, force_insert=False, force_update=False, using=None, update_fields=None):
        self.slug = slugify(self.product_name, allow_unicode=True)
        super().save(force_insert, force_update, using, update_fields)
```
>- slug 자동완성 기능을 쓰려면 파라미터에 allow_unicode=True 로 해둬도 가능하지만 그럴 경우 urls 에서 다시 한 번 작업해줘야 한다.
>	- model 의 save함수를 오버라이딩하면 모델에서 저장될 때마다 slug fields 값을 추가하도록 할 수 있다.
>- `images` ->카테고리 모델에서 이미지필드의 upload 장소를 베이스를 photos 로 해주었으니 동일하게 맞춰준다.

### admin.py
```python
from django.contrib import admin
from .models import Product

# Register your models here.

class ProductAdmin(admin.ModelAdmin):
    list_display = ('product_name', 'price', 'stock', 'category', 'modified_date', 'is_available')
    prepopulated_fields = {'slug': ('product_name',)}

admin.site.register(Product, ProductAdmin)
```

![](https://i.imgur.com/THEp2zl.png)

## 렌더링해주기

```python
from django.shortcuts import render
from store.models import Product

def home(request):
    products = Product.objects.all().filter(is_available = True)
    context = {
        'products': products,
    }
    return render(request, 'home.html',context)
```

```html
<div class="row">
    {% for product in products %}
    <div class="col-md-3">
        <div class="card card-product-grid">
            <a href="./product-detail.html" class="img-wrap"> <img src="{{product.images.url}}"> </a>
            <figcaption class="info-wrap">
                <a href="./product-detail.html" class="title">{{ product.product_name }}</a>
                <div class="price mt-1">{{ product.price }}</div> <!-- price-wrap.// -->
            </figcaption>
        </div>
    </div>
    {% endfor %}
</div> <!-- row.// -->
```

![](https://i.imgur.com/50Q6CBl.png)

## 숫자 포맷 적용시키기

- 장고에는 숫자나 날씨 포맷이 이미 내장되어있다.
### settings.py
```python
INSTALLED_APPS = [
    'django.contrib.humanize', # 숫자 포멧
]
```
>- 따로 라이브러리 설치 필요 없이 그냥 `humanize` 를 앱에 등록해준다.

```python
{% load humanize %}

{{ product.price|intcomma}}
```
>- 그 후 static 폴더처럼 호출한 뒤 태그를 씌워주면 적용된다.
>- [참고]([django.contrib.humanize | Django documentation | Django (djangoproject.com)](https://docs.djangoproject.com/en/4.2/ref/contrib/humanize/))

![](https://i.imgur.com/xemdehQ.png)

## 페이지 연결시키기

### urls.py





```python
{% extends 'base.html' %}

{% load static %}

  

{% block content %}

  <!-- ========================= SECTION PAGETOP ========================= -->

  <section class="section-pagetop bg">

    <div class="container">

      <h2 class="title-page">Our Store</h2>

  

    </div>

    <!-- container // -->

  </section>

  <!-- ========================= SECTION INTRO END// ========================= -->

  <!-- ========================= SECTION CONTENT ========================= -->

  <section class="section-content padding-y">

    <div class="container">

  

      <div class="row">

        <aside class="col-md-3">

  

          <div class="card">

            <article class="filter-group">

              <header class="card-header">

                <a href="#" data-toggle="collapse" data-target="#collapse_1" aria-expanded="true" class="">

                  <i class="icon-control fa fa-chevron-down"></i>

                  <h6 class="title">Categories</h6>

                </a>

              </header>

              <div class="filter-content collapse show" id="collapse_1" style="">

                <div class="card-body">

  

                  <ul class="list-menu">

                    <li>

                      <a href="#">People

                      </a>

                    </li>

                    <li>

                      <a href="#">Watches

                      </a>

                    </li>

                    <li>

                      <a href="#">Cinema

                      </a>

                    </li>

                    <li>

                      <a href="#">Clothes

                      </a>

                    </li>

                    <li>

                      <a href="#">Home items

                      </a>

                    </li>

                    <li>

                      <a href="#">Animals</a>

                    </li>

                    <li>

                      <a href="#">People

                      </a>

                    </li>

                  </ul>

  

                </div>

                <!-- card-body.// -->

              </div>

            </article>

            <!-- filter-group .// -->

            <article class="filter-group">

              <header class="card-header">

                <a href="#" data-toggle="collapse" data-target="#collapse_4" aria-expanded="true" class="">

                  <i class="icon-control fa fa-chevron-down"></i>

                  <h6 class="title">Sizes

                  </h6>

                </a>

              </header>

              <div class="filter-content collapse show" id="collapse_4" style="">

                <div class="card-body">

                  <label class="checkbox-btn">

                    <input type="checkbox">

                    <span class="btn btn-light">

                      XS

                    </span>

                  </label>

  

                  <label class="checkbox-btn">

                    <input type="checkbox">

                    <span class="btn btn-light">

                      SM

                    </span>

                  </label>

  

                  <label class="checkbox-btn">

                    <input type="checkbox">

                    <span class="btn btn-light">

                      LG

                    </span>

                  </label>

  

                  <label class="checkbox-btn">

                    <input type="checkbox">

                    <span class="btn btn-light">

                      XXL

                    </span>

                  </label>

                </div>

                <!-- card-body.// -->

              </div>

            </article>

            <!-- filter-group .// -->

  

            <article class="filter-group">

              <header class="card-header">

                <a href="#" data-toggle="collapse" data-target="#collapse_3" aria-expanded="true" class="">

                  <i class="icon-control fa fa-chevron-down"></i>

                  <h6 class="title">Price range

                  </h6>

                </a>

              </header>

              <div class="filter-content collapse show" id="collapse_3" style="">

                <div class="card-body">

  

                  <div class="form-row">

                    <div class="form-group col-md-6">

                      <label>Min</label>

                      <!-- <input class="form-control" placeholder="$0" type="number"> -->

                      <select class="mr-2 form-control">

                        <option value="0">$0</option>

                        <option value="50">$50</option>

                        <option value="100">$100</option>

                        <option value="150">$150</option>

                        <option value="200">$200</option>

                        <option value="500">$500</option>

                        <option value="1000">$1000</option>

                      </select>

                    </div>

                    <div class="form-group text-right col-md-6">

                      <label>Max</label>

                      <select class="mr-2 form-control">

                        <option value="50">$50</option>

                        <option value="100">$100</option>

                        <option value="150">$150</option>

                        <option value="200">$200</option>

                        <option value="500">$500</option>

                        <option value="1000">$1000</option>

                        <option value="2000">$2000+</option>

                      </select>

                    </div>

                  </div>

                  <!-- form-row.// -->

                  <button class="btn btn-block btn-primary">Apply</button>

                </div>

                <!-- card-body.// -->

              </div>

            </article>

            <!-- filter-group .// -->

  

          </div>

          <!-- card.// -->

  

        </aside>

        <!-- col.// -->

        <main class="col-md-9">

  

          <header class="border-bottom mb-4 pb-3">

            <div class="form-inline">

              <span class="mr-md-auto">32 Items found

              </span>

  

            </div>

          </header>

          <!-- sect-heading -->

  

          <div class="row">

            {% for product in products %}

            <div class="col-md-4">

              <figure class="card card-product-grid">

                <div class="img-wrap">

  

                  <img src="images/items/1.jpg">

  

                </div>

                <!-- img-wrap.// -->

                <figcaption class="info-wrap">

                  <div class="fix-height">

                    <a href="./product-detail.html" class="title">Great item name goes here</a>

                    <div class="price-wrap mt-2">

                      <span class="price">$1280</span>

                      <del class="price-old">$1980</del>

                    </div>

                    <!-- price-wrap.// -->

                  </div>

                  <a href="#" class="btn btn-block btn-success">Added to cart

                  </a>

                </figcaption>

              </figure>

            </div>

            <!-- col.// -->

            {% endfor %}

          </div>

          <!-- row end.// -->

  

          <nav class="mt-4" aria-label="Page navigation sample">

            <ul class="pagination">

              <li class="page-item disabled">

                <a class="page-link" href="#">Previous</a>

              </li>

              <li class="page-item active">

                <a class="page-link" href="#">1</a>

              </li>

              <li class="page-item">

                <a class="page-link" href="#">2</a>

              </li>

              <li class="page-item">

                <a class="page-link" href="#">3</a>

              </li>

              <li class="page-item">

                <a class="page-link" href="#">Next</a>

              </li>

            </ul>

          </nav>

  

        </main>

        <!-- col.// -->

  

      </div>

  

    </div>

    <!-- container .// -->

  </section>

  <!-- ========================= SECTION CONTENT END// ========================= -->

  

{% endblock %}
```





{% endraw %}
