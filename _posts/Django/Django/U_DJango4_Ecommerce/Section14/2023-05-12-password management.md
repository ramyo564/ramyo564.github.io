---
layout: single
title: " [Django] password management "
categories: Django
tags:
  - Python
  - Password
  - management
toc: true
toc_sticky: true
author_profile: false
sidebar:
---
# Password management 

/ 비밀번호 변경 /

## Creating the password reset urls.py
{% raw %}
```python
from django.urls import path
from . import views
from django.contrib.auth import views as auth_views


# Password management urls/views

# 1) Submit our email form
path('reset_password', auth_views.PasswordResetView.as_view(template_name='account/password/password-reset.html')
	, name='reset_password'),

# 2) Success management stating that a password reset email was sent
path('reset_password_sent', auth_views.PasswordResetDoneView.as_view(template_name='account/password/password-reset-sent.html')
	, name='password_reset_done'),

# 3) Password reset link
path('reset/<uidb64>/<token>/', auth_views.PasswordResetConfirmView.as_view(template_name='account/password/password-reset-form.html')
	, name='password_reset_confirm'),

# 4) Success mesasge stating that our password was reset
path('reset_password_complete', auth_views.PasswordResetCompleteView.as_view(template_name='account/password/password-reset-complete.html')
	, name='password_reset_complete'),

```
{% endraw %}


>- auth_views 를 통해서 아주 편하게 구현 가능하다.
>- 이메일 인증과 기본적으로 로직은 비슷하다.


![](https://i.imgur.com/bezEReg.png)


![](https://i.imgur.com/rdLXzlu.png)




![](https://i.imgur.com/DKl2uik.png)


![](https://i.imgur.com/4wyQcjF.png)




![](https://i.imgur.com/Xz3eMwj.png)






