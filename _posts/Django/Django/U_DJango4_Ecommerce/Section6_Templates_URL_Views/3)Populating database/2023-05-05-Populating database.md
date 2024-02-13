---
layout: single
title: " [Django] Populating database "
categories: Django
tags:
  - Python
  - Basic
  - Populating
  - database
toc: true
toc_sticky: true
author_profile: false
sidebar:
---
# Basic templates, URL's and VIews (3)
/ Populating database /

## Populating database
- 이전에 만들어 두었던 Category 모델과 Product 모델은 아직 연결이 안되어 있다.
### 
```python
class Product(models.Model):
    category = models.ForeignKey(Category, related_name='product', on_delete=models.CASCADE, null=True)
```
>- `on_delete=models.CASCADE`  FK를 포함하는 모델 인스턴스(row)도 같이 삭제된다.
>	- 카테고리에 신발이 있는데 신발 카테고리안에 나이키 에어맥스 상품이 있다고 할 때
>	- 신발 카테고리를 삭제하면 나이키 에어맥스도 삭제한다는 뜻
>- `python manage.py makemigrations`
>- `python manage.py migrate`
>	- 모델 및 DB에 대해서 변경사항이 있으니 마이그레이션을 해줘야한다.
>- ![](https://i.imgur.com/mNqICbT.png)

>- 참고 : https://moondol-ai.tistory.com/411
>- 이제 Products에서 카테고리를 요구하는게 잘 작동되는지 확인하면 끝
>- ![](https://i.imgur.com/RW2wQjg.png)


