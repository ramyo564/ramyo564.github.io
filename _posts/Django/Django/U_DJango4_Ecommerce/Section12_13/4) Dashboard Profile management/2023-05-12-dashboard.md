---

layout: single
title: " [Django] dashboard "
categories: Django
tag: [Python,"[BIG][Django] User management","파이썬함수if문 raise에러","[Django] dashboard, login_required"]
toc: true
toc_sticky: true
author_profile: false
sidebar:

---
# User management (4)
{% raw %}
/ raise / login_required / dashboard


## Dashboard

```python
from django.contrib.auth.decorators import login_required

@login_required(login_url='my-login')
def dashboard(request):
    return render(request, 'account/dashboard.html')
```

>- 로그인을 했을 경우 dashboard로 안했을 경우 my-login 페이지로 이동
>- login_required 를 구현하는 방법은 3가지가 있다.


###  **1) request.user로 판단**

```python
def index(request: HttpRequest) -> HttpResponse:    
	if not request.user.is_authenticated:        
		return redirect("/accounts/login/")    
	qs = Post.objects.all()    
	return render(request, "blog/index.html", context={"post_list": qs})
```

index함수는 view의 함수로 request를 input으로 받습니다. user가 로그인된 경우, request.user.is_authenticated는 True를 반환합니다. 이를 이용해서 조건문으로 로그인되지 않은 경우 login 화면으로 리다이렉트 하는 코드

![](https://i.imgur.com/q0V2YkG.png)


### **2) @login_required로 판단**

데코레이터 문법을 활용한 방법
데코레이터를 활용한 방법을 AOP(Aspect Oriented Programming:관점 지향 프로그래밍)
AOP는 쉽게말해, 보기 좋게 프로그래밍하는 것
데코레이터로 함수/클래스를 감싸는 형태로 필요한 기능을 수행하기 때문

```python
@login_required
def post_detail(request: HttpRequest, pk: int) -> HttpResponse:    
	# post = Post.objects.get(pk=pk)    
	post = get_object_or_404(Post, pk=pk)  
	# 해당 오브젝트가 없으면 404페이지로 보낸다.    
	return render(request, "blog/post_detail.html", {"post": post})
```

데코레이터를 이용한 방법 
View에서 사용하고자 하는 함수위에 @login_required를 작성해주면 그 바로 아래의 함수에 대해 wrapper(래퍼) 역할을 하여 로그인된 경우만 아래 함수가 실행될 수 있도록 하고, 로그인하지 않은 상태로 아래 함수를 호출하게 되면 해당 유저는 로그인 화면으로 가게 된다. 
(클래스형 뷰에는 이를 쓸 수 없다.)

![](https://i.imgur.com/o8QeeQC.png)


### **3) login_required() 함수 사용**

```python
post_new = login_required(
	CreateView.as_view(        
		model=Post,       
		form_class=PostForm,        
		success_url="/blog/",
	)
)
```

CreateView.as_view()라는 CBV(Class Based View)를 사용하는 경우는 함수/클래스를 정의하는 게 아니고, 정의된 클래스를 import 하여 사용하기 때문에 데코레이터를 사용하기 어렵다.
그럴 땐, login_required() 함수 내부에 CreateView.as_view() 함수를 작성하면, 해당 뷰 함수는 로그인된 경우만 호출



![](https://i.imgur.com/dUP1Z43.png)
>- 참고 : https://bio-info.tistory.com/172

## Profile

### Update foam 만들기 account > foams.py
```python
# Update form
class UpdateUserForm(forms.ModelForm):
    password = None
    class Meta:
        model = User
        fields = ['username', 'email']
        exclude = ['password1','password2']
    def __init__(self, *args, **kwargs):
        super(UpdateUserForm, self).__init__(*args, **kwargs)

        # Maek email field as required
        self.fields['email'].required= True
        
        # Email validation
    def clean_email(self):
        email = self.cleaned_data.get('email')
        if User.objects.filter(email=email).exclude(pk=self.instance.pk).exists():
            raise forms.ValidationError('This email is invalid')

        # Len function updated
        if len(email) >= 350:
            raise forms.ValidationError('Your email is too long')
        return email
```
>- raise 조건문에 안 맞으면 에러 발생
>- 비밀번호는 Forgotten your password로 대체한다.
>- 이메일은 주민번호처럼 고유값이므로 유효성 검사를 통해 중복이 되지 않도록 만들어준다.

![](https://i.imgur.com/ws6zvXj.png)


![](https://i.imgur.com/UT0cps0.png)


### 뷰 연결하기 account > views.py
```python
@login_required(login_url='my-login')
def profile_management(request):

    # Updating our user's username and email
    user_form = UpdateUserForm(instance=request.user)
    if request.method == 'POST':
        user_form = UpdateUserForm(request.POST, instance=request.user)
        
        if user_form.is_valid():
            user_form.save()
            messages.info(request, "Account updated")
            return redirect('dashboard')

    context = {'user_form':user_form}
    return render(request, 'account/profile-management.html', context=context)
```
>- 업데이트 했을때 이메일로 유효성 검사가 완료된 경우에 변경된 내용을 저장해주고 대쉬보드로 리다이렉 해준다.


## Delete

### 뷰 연결하기 views.py

```python
@login_required(login_url='my-login')
def delete_account(request):
    user = User.objects.get(id=request.user.id)
    if request.method == 'POST':
        user.delete()
        messages.error(request, "Your account deleted")
        return redirect('store')

    return render(request, 'account/delete-account.html')
```






{% endraw %}
