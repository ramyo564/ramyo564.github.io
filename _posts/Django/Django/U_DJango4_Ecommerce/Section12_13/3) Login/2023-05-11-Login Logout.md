---

layout: single
title: " [Django] Login "
categories: Django
tag: [Python,"[BIG][Django] User management","[Django] Login and Logout"]
toc: true
toc_sticky: true
author_profile: false
sidebar:

---
# User management (3)
{% raw %}
/ Login / Logout

## Login & Logout
- Register를 통해 만든 아이디와 패스워드에 대한 폼을 만들어줘야한다.

### 로그인폼 만들기 account> forms.py
```python
from django.contrib.auth.forms import UserCreationForm, AuthenticationForm
from django.contrib.auth.models import User
from django import forms
from django.forms.widgets import PasswordInput, TextInput


# Login form
class LoginForm(AuthenticationForm):
    username = forms.CharField(widget=TextInput())
    password = forms.CharField(widget=PasswordInput())
```
>- AuthenticationForm 을 상속 받아서 사용한다.
>- 위젯을 사용하면 비밀번호 부분은 텍스트가 가려지며 우리가 일반적으로 아는  `****` 식으로 표현된다.


### 뷰에 연결하기 views.py
```python
from .forms import CreateUserForm, LoginForm, UpdateUserForm
from django.contrib.auth.models import auth
from django.contrib.auth import authenticate, login, logout
from django.contrib.auth.decorators import login_required

# Login
def my_login(request):
    form = LoginForm()
    if request.method == 'POST':
        form = LoginForm(request, data=request.POST)
        if form.is_valid():
            username = request.POST.get('username')
            password = request.POST.get('password')
            user = authenticate(request, username=username, password=password)
            if user is not None:
                auth.login(request, user)
                return redirect("dashboard")
    context = {'form':form}
    return render(request, 'account/my-login.html', context=context)

# Logout
def user_logout(request):
    try:
        for key in list(request.session.keys()):
            if key == "session_key":
                auth.logout(request)
                continue
	        else:
	            del request.session[key]
    except KeyError:
        pass
    messages.success(request, "Logout success")
    return redirect("store")
```
>- authenticate 를 이용해서 유저가 맞으면 대쉬보드로 보내준다.
>- auth를 통해 로그인 정보가 안 맞을 경우 알아서 경고창을 출력해준다.
>- 로그아웃 할 경우세션키가 있으면 그대로 유지해준다.
>- 정상적으로 로그아웃을 할 경우 session_key는 있겠지만 혹시 모르니 없으면 삭제해주고 에러가 날 경우 pass 해준다.




### urls 연결하기

```python
# Login / Logout URL's
path('my-login/', views.my_login, name='my-login'),
path('user-logout', views.user_logout, name='user-logout'),
```


## 네비게이션바 로그인 로그아웃 반응형을 만들기
```python
<div class="collapse navbar-collapse text-center" id="navbarNavDropdown">
	<ul class="navbar-nav ms-auto">
		{% if user.is_authenticated %}
			<li class="nav-item">
				<a class="btn btn-alert navbar-btn text-white" type="button"  href="{% url 'dashboard' %}">
					<i class="fa-regular fa-lightbulb"></i> &nbsp; Dashboard </a>
			</li>
			<li class="nav-item">
				<a class="btn btn-alert navbar-btn text-white" type="button"  href="{% url 'user-logout' %}"> <i class="fa-solid fa-arrow-right-from-bracket"></i>  &nbsp; Logout </a>
			</li>
			&nbsp; &nbsp; &nbsp;
		{% else %}
		<li class="nav-item">
			<a class="btn btn-alert navbar-btn text-white" type="button"  href="{% url 'register' %}"> Register </a>
		</li>
		<li class="nav-item">
			<a class="btn btn-alert navbar-btn text-white" type="button"  href="{% url 'my-login' %}"> Login </a>
		</li>
		&nbsp; &nbsp; &nbsp;
		{% endif %}
```

- ![](https://i.imgur.com/fd4DyC8.png)
- ![](https://i.imgur.com/3d3s6eX.png)
- 사실 별거 없고 조건문으로 로그인이 안 되어 있는 상태면 위의 창을
- 로그인이 되어 있다면 아래창을 출력하도록 조건문을 만들면 된다.


{% endraw %}
