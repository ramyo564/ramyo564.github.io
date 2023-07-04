---

layout: single
title: " [Django] Configureing Django for static and image uploads "
categories: Django
tag: [Python,"[BIG][Django] Building models","[Django] static 폴더 경로 설정"]
toc: true
toc_sticky: true
author_profile: false
sidebar:

---
# Building models (3)

/ Configureing Django for static / Configureing Django for image uploads

## Configureing Django for static

- 플라스크와 동일하게 static 폴더를 만들어준다.
- ![](https://i.imgur.com/xdELzs5.png)
- 그후 static 베이스 경로를 설정해준다.
### settings.py
```python
STATIC_URL = '/static/'
STATICFILES_DIRS = [BASE_DIR / 'static']
```


## Configuring Django for image uploads

- static 환경설정과 비슷하다.
- 정적 파일을 올리기 위해서는 urls.py에서 설정을 해주면 된다.
### settings.py
```python
MEDIA_URL = '/media/'
MEDIA_ROOT = BASE_DIR / 'static/media'
```

### urls.py
```python
from django.contrib import admin
from django.urls import path

from django.conf import settings
from django.conf.urls.static import static

urlpatterns = [
    path('admin/', admin.site.urls),
]

urlpatterns += static(settings.MEDIA_URL, document_root = settings.MEDIA_ROOT)
```
>- if we were to access an image that we upload so remember, we have here in our store models, we defined an image attribute.
>- So what's going to happen is that it says upload two images.
>- We already defined a media folder.
>- So in this media folder, when we upload an image, what's going to happen is that the first time we upload an image is going to create a directory called images, which is going to be in our media folder. And from then on, all images that are uploaded to Django will be in media and then slash images.
>- It has connected our images to our Django application by setting the ULS py accordingly.
>- So our unique image URL and we've also gone ahead and just configured those settings that we need to actually connect and to actually upload images appropriately and connect the parts.

추가 참고 : https://blog.hannal.com/2015/04/start_with_django_webframework_06/

>- 정적파일로 운영하는게 더 빠른지 아니면 온라인으로 이미지 호스팅 방식으로 운영하는것 중 어떤게 더 빠른지는 아직 잘 모르겠다
>- Django 에서는 정적 파일 제공 기능을 제공하는데 일반적으로 성능은 웹 서버가 직접 정적 파일을 제공 하는 것보다 떨어지지만 정적 파일 제공에 필요한 기능은 대부분 지원한다고 한다.