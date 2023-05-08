---

layout: single
title: " [Django] Cart functionality Add "
categories: Django
tag: [Python,"[BIG][Django] Cart functionality",[Django] Cart Add]
toc: true
toc_sticky: true
author_profile: false
sidebar:

---
# Cart(3)
{% raw %}
/ !!! /

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









## Update the cart summary template
















{% endraw %}
