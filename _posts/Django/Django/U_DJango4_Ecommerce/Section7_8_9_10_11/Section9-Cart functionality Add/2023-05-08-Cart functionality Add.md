---

layout: single
title: " [Django] Cart functionality Add "
categories: Django
tag: [Python,"[BIG][Django] Cart functionality","[Django] Cart Add"]
toc: true
toc_sticky: true
author_profile: false
sidebar:

---
# Cart (3)
{% raw %}
/ 장바구니 만들기 /

## AJAX integration - Add

- Product-info 에서 장바구니 버튼을 눌렀을 때 value에 상품 id 값을 넣어줌

### product-info.html
```python
<button type="button" id="add-button" value="{{product.id}}" class="btn btn-secondary btn-sm">
Add to cart
</button>


...

$(document).on('click', '#add-button', function (e) {
	e.preventDefault();
	$.ajax({
	  type: 'POST',
	  url: '{% url "cart-add" %}',
	  data: {
		product_id: $('#add-button').val(),
		product_quantity: $('#result').text(),
		csrfmiddlewaretoken: "{{csrf_token}}",
		action: 'post' 
	},
	success: function (json) {
	  console.log(json)
	  
	},
	error: function (xhr, errmsg, err) {}
  });
})
```

>- urls.py 에 `path('add/', views.cart_add, name='cart-add'),` 를 추가해주고 name 값 'cart-add' 를 ajax url에 연결해준다.
>- product_id 는 위에서 지정해준 `product.id` 값을 갖고 온다.
>- product_quantity 는 이전에 수량 변화에 대한 id 값 result를 넣고 text 값으로 갖고 오면 된다.
>- 장고에서는 어차피 CSRF 토큰을 기본적으로 사용하니 미리 발급해준다.
>- 참고 : https://chagokx2.tistory.com/49
 

### cart>views.py

```python
from django.shortcuts import render
from .cart import Cart
from store.models import Product
from django.shortcuts import get_object_or_404
from django.http import JsonResponse

def cart_add(request):
    cart = Cart(request)
    if request.POST.get('action') == 'post': # match lower case in action from product-info.html
        product_id = int(request.POST.get('product_id'))
        product_quantity = int(request.POST.get('product_quantity'))
        product = get_object_or_404(Product, id=product_id)
        cart.add(product=product, product_qty=product_quantity)
        
        # session updating in realtime when you click the button
        cart_quantity = cart.__len__()
        response = JsonResponse({'product titke': product.title, 'quantity':product_quantity})

        return response
```
>- cart.py 에 있는 Cart 클래스를 불러준다.
>- `get_object_or_404` 로 유효성 검사를 해준다. 해당 product_id 값이 Product에 있으면 cart의 add 함수를 불러와서 코드가 실행된다.
>	- cart에서 add 함수를 불러와야 하므로 cart.py에 add 함수를 만들어준다.

### cart.py
```python
class Cart():
...
def add(self, product, product_qty):
        product_id = str(product.id)
        if product_id in self.cart:
            self.cart[product_id]['qty'] = product_qty
        else:
            self.cart[product_id] = {'price': str(product.price), 'qty': product_qty}
        self.session.modified = True # session update
```
>- 아이템을 장바구니에 추가 했을 때 이미 장바구니에 해당 아이템이 있다면 가격정부는 이미 있으므로 굳이 가격 정보를 또 넣어줄 필요가 없다
>	- 이 때는 수량만 반영해주면 된다.
>- 반면 해당 아이템이 장바구니에 없다면 아무런 정보가 없으니 가격과 수량 모두 반영해주면 된다.


![](https://i.imgur.com/JtKM2CF.png)
>- 수량과 이름 모두 잘 반영되는걸 콘솔에서 확인하면 끝


### 장바구니에 숫자 표시해주기
![](https://i.imgur.com/C4lFXoI.png)
- 현재 장바구니에는 딱히 표시되는게 없는데 장바구니에 아이템이 추가되면 아이템 갯수만큼 반영되도록 해보자.



#### base.html
```python
<a type="button" role="button" href="{% url 'cart-summary' %}" class="btn btn-outline-secondary">
	<i class="fa fa-shopping-cart" aria-hidden="true"> </i>
&nbsp;
	<div id="cart-qty" class='d-inline-flex '>
		0
	</div>
</a>
```
>- ![](https://i.imgur.com/EhjPajN.png)
>- 장바구니에 숫자 표시

### cart > views.py
```python
def cart_add(request):
    cart = Cart(request)
    if request.POST.get('action') == 'post': # match lower case in action from product-info.html
        product_id = int(request.POST.get('product_id'))
        product_quantity = int(request.POST.get('product_quantity'))
        product = get_object_or_404(Product, id=product_id)
        cart.add(product=product, product_qty=product_quantity)
        
        # response = JsonResponse({'product titke': product.title, 'quantity':product_quantity})
        response = JsonResponse({'qty': product_quantity})
        return response
```
>- 장바구니에서 상품 이름은 필요 없으므로 qty 라는 키 값으로 상품 수량을 장바구니에 반영해준다.


### cart > product-info.html
```python
success: function (json) {
  //console.log(json)
  document.getElementById("cart-qty")
  .textContent = json.qty 
  
  // need to be matched with JsonResponse dictionary key in cart_add function from view.py

},
```
>- `document.getElementById("cart-qty").textContent = json.qty` 는 base.html 에서 id 값 `cart-qty` 의 텍스트 값이  views.py 의 키 값 'qty' 의 벨류 값으로 반영됨
>- ![](https://i.imgur.com/D6WCUfs.png)
>-  ![](https://i.imgur.com/kQFk4Jk.png)
>- 하지만 새로고침을 하면 데이터가 날아간다.
>- 따라서 세션에 저장된 데이터(수량) 가 계속 페이지에 반영되도록 작업을 해줘야한다.


### cart.py
```python
def __len__(self):
	try:
		return sum(item['qty'] for item in self.cart.values())
	except:
		return 0
```
>- self.cart 딕셔너리에 담겨 있는 키 값은 product의 id 값이다.
>	- 예를들어 티쳐츠는 id 1번 팬츠는 2번 이런식
>	- 따라서 values 값으로 호출을 해야한다. -> `[{'price':'45000', 'qty':2}, {'price':'60000', 'qty':3}]` 
>	- 해당 값의 키값 qty로 호출
>	- 값이 없으면 0으로 리턴

#### base.html
```python
<div id="cart-qty" class='d-inline-flex '>
	{% with qty_amount=cart|length %}
		{% if qty_amount > 0 %}
			{{ qty_amount }}
		{% else %}
			0
		{% endif %}
	{% endwith %}
</div>
```
>- 전역으로 사용할 수 있겠금 TEMPLATES에 cart 를 등록해줬으므로 with 템플릿 태그를 사용해서 length를 호출해준다.
>- qty_amount 가 0보다 크면 qty_amount를 출력해주고 아니면 0을 출력
>	- Wtih statement 참고 : https://jinja.palletsprojects.com/en/3.1.x/templates/


### Issue

![](https://i.imgur.com/Ttph5AP.png)
- 이전에 티셔츠 상품을 방구니에 넣고 새로운 상품을 넣으면 현재 상품 갯수만 반영되고 합산이 안된다.
- ![](https://i.imgur.com/5H1TM0o.png)
- 여기서 새로고침을 눌러줘야지만 제대로 반영이 된다.
- ![](https://i.imgur.com/w3eTBl0.png)
- 해당 부분을 해결해주기 위해서는 세션에서 전체수량을 넘겨줘야한다.

### cart>views.py
```python
def cart_add(request):
    cart = Cart(request)
    if request.POST.get('action') == 'post': # match lower case in action from product-info.html
        product_id = int(request.POST.get('product_id'))
        product_quantity = int(request.POST.get('product_quantity'))
        product = get_object_or_404(Product, id=product_id)
        cart.add(product=product, product_qty=product_quantity)
        # session updating in realtime when you click the button
        cart_quantity = cart.__len__()

        # response = JsonResponse({'qty': product_quantity})
		response = JsonResponse({'qty': cart_quantity})
  
        return response
```
>- `cart.__len__()` 으로 세션에 있는 데이터를 불러와서 cart_quantity 객체를 만들고 qty의 키값의 벨류로 넣어주면 실시간으로 업데이트가 잘 된다.

### cart.py
```python
    def add(self, product, product_qty):
        product_id = str(product.id)
        if product_id in self.cart:
            self.cart[product_id]['qty'] += product_qty
        else:
            self.cart[product_id] = {'price': str(product.price), 'qty': product_qty}
        self.session.modified = True # session update
```
>- 또한 이전에 `self.cart[product_id]['qty'] = product_qty` 부분을 += 로 중첩으로 바꿔줘야지 안 그러면 셔츠 2개 장바구니 넣고 팬츠 3개 넣었다가 다시 셔츠 1개를 장바구니에 넣으면 셔츠가 총 3개로 쌓이는게 아니라 다시 1개로 줄어든다.
>	- 논리를 잘 생각하면서 짜면 된다.






{% endraw %}
