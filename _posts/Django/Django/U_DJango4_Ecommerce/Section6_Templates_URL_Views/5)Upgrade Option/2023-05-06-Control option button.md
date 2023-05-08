---

layout: single
title: " [Django] Control option button "
categories: Django
tag: [Python,"[BIG][Django] Basic templates, URLS's and VIews","[Django] 숫자 형식 변환","[Django] 장바구니 클릭 옵션 제어"]
toc: true
toc_sticky: true
author_profile: false
sidebar:

---
# Basic templates, URLS's and VIews (5)
{% raw %}
/ 장바구니 옵션 버튼  / 숫자 형식 /


![](https://i.imgur.com/v1jjiII.png)

## 수량 옵션 클릭으로 변경하기

- 현재 상품페이지에는 샘플로 만든거라 하드코딩으로 되어 있다.
- 해당 부분을 일반적으로 사용하는 +,- 버튼으로 변경해보자.
![](https://i.imgur.com/T1lk8LQ.png)

```python
<div class="col-sm-10" style="text-align: center; width: 10px; display:flex; ">
	<input type='button'
		onclick='count("minus")'
		value='-'  class="form-control"/>
	&nbsp;&nbsp;
	
	<div id='result' class="form-control">1</div>
	&nbsp;&nbsp;
	
	<input type='button'
		onclick='count("plus")'
		value='+' class="form-control"/>
</div>


<script>
    function count(type)  {
        // 결과를 표시할 element
        const resultElement = document.getElementById('result');
        
        // 현재 화면에 표시된 값
        let number = resultElement.innerText;

        // 더하기/빼기
        if(type === 'plus') {
            number = parseInt(number) + 1;
        }else if(type === 'minus')  {
            number = parseInt(number) - 1;
        }

        // 결과 출력
        if(number > 0){
            resultElement.innerText = number;
        }
      }
</script>
```
>-  장바구니에서 음수가 나오지 않게 유효성 검사를 해준다.
>-  1으로 표시되는 div 태그에 id 값 result 를 변수 resultElement에 할당
>-  resultElement에는 현재 1값이 들어가 있고 최종 업데이트 때 1이하가 되면 작동 안되게 하면 끝
>	- 어차피 장바구니에 0개가 들어가는게 말이 안됨
>- 참고 : https://hianna.tistory.com/476

### 숫자 형식

![](https://i.imgur.com/GWV5ky7.png)

```python
INSTALLED_APPS = [
    'django.contrib.humanize', # 숫자 포멧
]
```

```python
{% load humanize %}
{{ product.price|intcomma}}
```
>- settings.py 에 'django.contrib.humanize' 를 추가해주고 해당 포맷이 필요한 템플릿 상단에 {% load humanize %} 를 삽입해준다.
>- 그리고 포맷 변환이 필요한 곳에 intcomma 를 넣어주면 끝
>- 참고 : https://velog.io/@joje/django-humanize-%ED%95%84%ED%84%B0-%EC%85%8B-%ED%99%9C%EC%9A%A9, https://kimdoky.github.io/django/2018/05/11/django-1000comma/

{% endraw %}


