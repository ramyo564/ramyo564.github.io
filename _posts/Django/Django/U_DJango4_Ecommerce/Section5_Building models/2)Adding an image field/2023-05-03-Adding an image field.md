---

layout: single
title: " [Django] Adding an image field "
categories: Django
tag: [Python,"[BIG][Django] Building models","[Django] Pillow","[Django] ImageField"]
toc: true
toc_sticky: true
author_profile: false
sidebar:

---
# Building models (2)

/ Pillow / ImageField /

- 이전에 추가한 ImageField 를 sqlite3와 연동하기 위해서는 Pillow 라이브러리를 설치해야하고 마이그레이션을 해준다.
## Adding an image field
```cmd
pip install Pillow

python manage.py makemigrations

python manage.py migrate
```


![](https://i.imgur.com/And1OGs.png)


