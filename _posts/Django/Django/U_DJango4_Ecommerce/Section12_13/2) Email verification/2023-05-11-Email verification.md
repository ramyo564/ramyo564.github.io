---

layout: single
title: " [Django] Email verification "
categories: Django
tag: [Python,"[BIG][Django] User management",'[Django] Email verification',"이메일 유효성 검사",'token','깃허브 SECRETS_KEY',"AbstractUser.email_user
","이메일 태그 가리기 autoescape"]
toc: true
toc_sticky: true
author_profile: false
sidebar:

---
# User management (2)
{% raw %}
/ 이메일 유효성 검사 / token / 깃허브로 SECRETS_KEY 관리하기 / AbstractUser.email_user

## Email verification
- 우선 경로설정부터 해준다.

### views.py
```python
def email_verification(request):
    pass
    
def email_verification_sent(request):
    return render (request,'account/registration/email-verification-sent.html')
    
def email_verification_success(request):
    return render (request,'account/registration/email-verification-success.html')
    
def email_verification_failed(request):
    return render (request,'account/registration/email-verification-failed.html')
```

### urls.py
```python
urlpatterns = [
    path('register/', views.register, name='register'),

    # # Email verification URL's
    path('email-verification/<str:uidb64>/<str:token>/', views.email_verification, name='email-verification'),
    path('email-verification-sent/', views.email_verification_sent, name='email-verification-sent'),
    path('email-verification-success/', views.email_verification_success, name='email-verification-success'),
    path('email-verification-failed/', views.email_verification_failed, name='email-verification-failed'),
    ]
```

## 이메일 유효성 검사
`pip install django-utils-six`
- 장고에서 더 이상 지원해주지 않아서 라이브러리를 추가 설치해줘야 한다.

### token.py

```python
# - Import password reset token generator
from django.contrib.auth.tokens import PasswordResetTokenGenerator
from django.utils import six

# - Password reset token generator method

class UserVerificationTokenGenerator(PasswordResetTokenGenerator):
    def _make_hash_value(self, user, timestamp):
        user_id = six.text_type(user.pk)
        ts = six.text_type(timestamp)
        is_active = six.text_type(user.is_active)
        return f"{user_id}{ts}{is_active}"

user_tokenizer_generate = UserVerificationTokenGenerator()
```
>- 비밀번호 재생성 토큰과 원리는 같다.
>- 제네레이터는 등록된 이메일로 유저에게 고유한 코드를 준다.
>- 발송된 이메일에서 접속한 후 클릭하면 그 때부터 활성화 된다.
>- 자세한 설명은 아래에서 한 번 더!
>-  참고 : https://ssungkang.tistory.com/entry/Django-%E1%84%92%E1%85%AC%E1%84%8B%E1%85%AF%E1%86%AB%E1%84%80%E1%85%A1%E1%84%8B%E1%85%B5%E1%86%B8-%E1%84%89%E1%85%B5-%E1%84%8B%E1%85%B5%E1%84%86%E1%85%A6%E1%84%8B%E1%85%B5%E1%86%AF-%E1%84%8B%E1%85%B5%E1%86%AB%E1%84%8C%E1%85%B3%E1%86%BC-SMTP
>


### account/views.py (register)
- 코드가 길어져서 register 함수랑 email_verification 나눠서 기록

```python
from django.shortcuts import render, redirect
from .forms import CreateUserForm
from django.contrib.sites.shortcuts import get_current_site
from .token import user_tokenizer_generate

from django.template.loader import render_to_string
from django.utils.encoding import force_str ,force_bytes
from django.utils.http import urlsafe_base64_decode, urlsafe_base64_encode

  
def register(request):
    form = CreateUserForm()
    if request.method == 'POST':
        form = CreateUserForm(request.POST)
        if form.is_valid():
            user = form.save()
            user.is_active = False
            user.save()

            # Email verification setup (template)
            current_site = get_current_site(request)
            subject = 'Account verification email'
            message = render_to_string('account/registration/email-verification.html', {
                'user' : user,
                'domain' : current_site.domain,
                'uid' : urlsafe_base64_encode(force_bytes(user.pk)),
                'token': user_tokenizer_generate.make_token(user),
            })
            user.email_user(subject=subject, message=message)
            return redirect('email-verification-sent')
    context = {'form':form}
    return render(request, 'account/registration/register.html', context=context)
```

>- Django 4 이전 버전은 `force_text` 로 사용하면 된다.
>- is_active() 함수는 좀 있다 User 모델을 통해 받아올 객체의 함수를 활성화 시키는 부분이다 -> 이메일로 인증이 완료되기 전에는 비활성화 상태로 만들어야 함으로 초기값을 False로 준다.
>- get_current_site()
>	- [`get_current_site()` 함수는](https://runebook.dev/ko/docs/django/ref/contrib/sites#django.contrib.sites.shortcuts.get_current_site) [`request.get_host()`](https://runebook.dev/ko/docs/django/ref/request-response#django.http.HttpRequest.get_host) 메서드 에서 [`domain`](https://runebook.dev/ko/docs/django/ref/contrib/sites#django.contrib.sites.models.Site.domain) 과 호스트 이름을 비교하여 현재 사이트를 가져오려고 시도 .
>	- 참고 : https://docs.djangoproject.com/en/4.2/ref/contrib/sites/
>- render_to_string()
>	- `render_to_string()` 은 [`get_template()`](https://runebook.dev/ko/docs/django/topics/templates#django.template.loader.get_template) 과 같은 템플릿을 로드하고 즉시 `render()` 메소드를 호출
>	- 참고 : https://docs.djangoproject.com/en/4.2/topics/templates/
>- user 는 User를 상속받는데 User는 AbstractUser를 상속받는다
>	- AbstractUser의 email_user 를 오버라이딩해서 사용한다.
>- 이메일을 보내주고 `email-verification-sent` 로 리다이렉트!

`email-verification-sent` 은 `email-verification-sent` html로 이메일을 확인하라는 창을 렌더링해준다.

이메일 발송은 아래와 같다.

### email-verification.html
```python
{% autoescape off %}

dasdsadasdasdasd <- message

http://{{domain}}{% url 'email-verification' uidb64=uid token=token %}

{% endautoescape %}
```
>- autoescape off -> html 코드를 가려주기 위해 켜둠
>- urls.py 에서는 아래와 같다.
>- `path('email-verification/<str:uidb64>/<str:token>/', views.email_verification, name='email-verification'),`
>	- uidb64와 token값 모두 string으로 받고 views에 있는 email_verification 로 이동

### account/views.py (email_verification)

```python
def email_verification(request, uidb64, token):
    # unique_id

    unique_id = force_str(urlsafe_base64_decode(uidb64))
    user = User.objects.get(pk=unique_id)

    # success
    if user and user_tokenizer_generate.check_token(user, token):
        user.is_active = True
        user.save()

        return redirect('email-verification-success')

    # failed
    else:
        return redirect('email-verification-failed')
```

>- pk가 email을 통해 받은 uidb64를 디코딩한 값과 일치하는 유저를 객체 user에 저장
>-  토큰과 user를 체크한 값과 user 의 값이 일치하면 user 활성화 해주고 활성화한 값을 save로 반영해줌 그리고 유효성 성공했다고 페이지 보여줌
>- 값이 다르면 실패했다고 알려줌

#### 암호화 과정

`user.pk` 를 암호화하는 과정은

현재 user.pk 는 자연수값이다. 이를 `force_byte` 를 통해 bytes 로 바꿔준다. 다음으로 `urlsafe_base64_encode` 로 인코딩해주고 이를 다시 `decode` 메소드를 통해 bytes에서 str 로 바꿔준다.

>- user.pk: 57 force_bytes(user.pk): b'57' 
>- urlsafe_base64_encode(force_bytes(user.pk)): b'NTc' 
>- urlsafe_base64_encode(force_bytes(user.pk)).decode(): NTc


위에 tokens.py 에서
`UserVerificationTokenGenerator` 클래스는 `PasswordResetTokenGenerator` 를 상속 받는다. `PasswordResetTokenGenerator` 는 장고에서 제공해주는 class 로서 비밀번호를 리셋할 떄 token을 발급해주고 해당 기능을 처리해준다. 
`text_type` 은 유니코드 정수로부터 유니코드 문자열을 가져온다. user의 pk, 현재시간, user의 활성화를 가지고 합쳐 tokens 을 생성.


## settings.py 이메일 설정

- 이메일을 보내주기 위한 설정 -> 로컬이 아닌 깃허브에서 관리할 키 값을 따로 관리하자.

```python
from pathlib import Path
import json
import os
from pathlib import Path
from django.core.exceptions import ImproperlyConfigured

# Build paths inside the project like this: BASE_DIR / 'subdir'.
BASE_DIR = Path(__file__).resolve().parent.parent

# Secret_KEY
secret_file = os.path.join(BASE_DIR, 'secrets.json')
with open(secret_file) as f:
    secrets = json.loads(f.read())

def get_secret(setting, secrets=secrets):
    try:
        return secrets[setting]
    except KeyError:
        error_msg = "Set the {} environment variable".format(setting)
        raise ImproperlyConfigured(error_msg)

MY_EMAIL = get_secret("MY_EMAIL")
EMAIL_PASSWORD = get_secret("EMAIL_PASSWORD")


# Email configuration settings:

EMAIL_BACKEND = 'django.core.mail.backends.smtp.EmailBackend'
EMAIL_HOST = 'smtp.gmail.com'
EMAIL_HOST_USER = MY_EMAIL
EMAIL_HOST_PASSWORD = EMAIL_PASSWORD
EMAIL_PORT = '587'
EMAIL_USE_TLS = 'True'
```
>- 요약하자면 경로를 설정해주고 secrets.json 파일을 만들어준 다음 해당 파일은 gitignore에 등록해준다.
>- 그리고 secrets.json 파일에서 딕셔너리 형태로 키값으로 벨류 값을 객체에 저장한 후 필요한 부분에 넣어주면 완료














{% endraw %}
