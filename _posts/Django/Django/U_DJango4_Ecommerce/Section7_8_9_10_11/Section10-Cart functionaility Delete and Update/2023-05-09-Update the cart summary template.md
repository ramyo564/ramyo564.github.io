---

layout: single
title: " [Django] Cart functionality Add Summary info "
categories: Django
tag: [Python,"[BIG][Django] Cart functionality","[Django] Cart Add info template"]
toc: true
toc_sticky: true
author_profile: false
sidebar:

---
# Cart (4)
{% raw %}
/ 장바구니 정보 페이지 /

## Update the cart summary template

### cart.py
```python
def __iter__(self):
	all_product_id = self.cart.keys()
	products = Product.objects.filter(id__in=all_product_id)
	cart = self.cart.copy()
	for product in products:
		cart[str(product.id)]['product'] = product
	for item in cart.values():
		item['price'] = int(item['price'])
		item['total'] = item['price'] * item['qty']
		yield item
```

>- 장바구니 페이지에서 cart를 호출하면 루프로 돌릴 수 있도록 이터레이터를 만들어준다.
>- 현재 상품 key 값이 상품 id값이고 장바구니에 있닌 id 값으로만 필터링해서 products 객체에 넣는다.
>- products 반복문으로 cart에 상품 id랑 제품명을 넣어준후
>- 제네레이터를 사용해 함수가 종료되지 않으면서 반복적으로 출력하게 만들어 준다.


### cart-summary.html
```python
{% for item in cart %}
{% with product=item.product %}


{% endwith %}
{% endfor %}

```

>- with 문으로 객체명을 바꿔준뒤 페이지를 꾸며주면 된다.

## 장바구니 계산 적용하기

![](https://i.imgur.com/62fvgh7.png)


>- 현재 수량은 12개지만 스포츠웨어 클럽의 단가만 나오고 수량 X 단가 가격으로 안나오고 있다.
>- 이를 해결하기 위해서는 django-mathfilters 라이브러리를 사용하면 된다.
>- `pip install django-mathfilters` 설치해준다.
>	- 자세한 설명은 https://pypi.org/project/django-mathfilters/
>- settings.py 에 앱에 mathfilters를 등록해주고
>- 템플릿에서 {% load mathfilters %} 부르고 필터를 써주면 끝이다.
>- ![](https://i.imgur.com/lNx8OIw.png)



{% endraw %}
