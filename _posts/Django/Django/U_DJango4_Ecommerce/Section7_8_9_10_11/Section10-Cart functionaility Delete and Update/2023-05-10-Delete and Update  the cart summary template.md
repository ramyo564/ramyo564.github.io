---

layout: single
title: " [Django] Cart functionality - Delete and Update "
categories: Django
tag: [Python,"[BIG][Django] Cart functionality","[Django] Cart functionality - Delete and Update","파이썬함수del","data-index"]
toc: true
toc_sticky: true
author_profile: false
sidebar:

---
# Cart (5)
{% raw %}
/ Cart info update  / Cart info delete / del / data-index

## Cart functionality - Delete and Update

### Delete
- 버튼을 클릭하면 상품을 삭제하도록 만들기

#### cart-summary-html
```python
<button type="button" data-index="{{product.id}}"
class="btn btn-danger btn-sm delete-button">
Delete
</button>

<script>
$(document).on('click', '.delete-button', function (e) {
  e.preventDefault();
  $.ajax({
	type: 'POST',
	url: '{% url "cart-delete" %}',
	data: {
	  product_id: $(this).data('index'), // index in session
	  csrfmiddlewaretoken: "{{csrf_token}}",
	  action: 'post'
		},
  success: function (json) {
	location.reload(); // refresh current page
	document.getElementById("cart-qty").textContent = json.qty 
	document.getElementById("total").textContent = json.total
  },
  error: function (xhr, errmsg, err) {
  }
	});
})
</script>
```

>- 아이템이 하나인 경우 장바구니에 넣는 것처럼 데이터 접근을 `value="{{product.id}}"` 로 해도 되지만 현재는 데이터가 여러개가 될 수 있으므로 `data-index` 로 접근해야 편하다.
>- 어떤 버튼에서 삭제 버튼을 누르는지 알아내려면 index로 갖고오면 value로 갖고 오는 것 보다 훨씬 편하게 할 수 있다.
>- url 은 urls.py name 과 연결해주면 끝
>- `location.reload();` 를 해줘야 클릭하자마자 바로 반영되는 것처럼 보임
>  그렇지 않으면 따로 새로고침을 하거나 페이지에서 벗어나야지 적용되는걸로 보임

#### cart>views.py
```python
def cart_delete(request):
    cart = Cart(request)
    if request.POST.get('action') == 'post':
        product_id = int(request.POST.get('product_id'))
        cart.delete(product = product_id)
        cart_quantity = cart.__len__()
        cart_total = cart.get_total()
        response = JsonResponse({'qty':cart_quantity, 'total': cart_total})
        return response
```
>- product id로 필터링해서 삭제한 후
>- 현재 남아있는 장바구니 데이터를 리턴해주면 된다.

#### cart.py
```python
class Cart():
    def delete(self, product):
        product_id = str(product)
        if product_id in self.cart:
            del self.cart[product_id]
        self.session.modified = True
```
>- 위에 요청을 수행하기 위해 클래서 Cart 에 delete 함수를 만들어 준다.
>- 마지막으로 세션 수정만 잊지말자

### Update

#### cart-summary-html
```python
<button type="button" data-index="{{product.id}}"
class="btn btn-primary btn-sm update-button">
Update
</button>

<script>
    // 업데이트 버튼
    $(document).on('click', '.update-button', function (e) {
      e.preventDefault();
      var theproductid = $(this).data('index');
      $.ajax({
        type: 'POST',
        url: '{% url "cart-update" %}',
        data: {
          product_id: $(this).data('index'),
          product_quantity: $('#select' + theproductid + ' option:selected').text(), // without empthy infornt of option makes error
          csrfmiddlewaretoken: "{{csrf_token}}",
          action: 'post'
              },
      success: function (json) {
        location.reload(); // refresh current page
        document.getElementById("cart-qty").textContent = json.qty 
        document.getElementById("total").textContent = json.total
      },
      error: function (xhr, errmsg, err) {
      }
          });
      })
</script>
```
>- 업데이트 된 수량에대해 반영해주는거 이외에는 별로 특별한게 따로 없다.

#### cart>views.py
```python
def cart_update(request):
    cart = Cart(request)
    if request.POST.get('action') == 'post':
        product_id = int(request.POST.get('product_id'))
        product_quantity = int(request.POST.get('product_quantity'))
        cart.update(product=product_id, qty=product_quantity)
        cart_quantity = cart.__len__()
        cart_total = cart.get_total()
        response = JsonResponse({'qty':cart_quantity, 'total': cart_total})
        return response
```

#### cart.py
```python
class Cart():
    def update(self, product, qty):
        product_id = str(product)
        product_quantity = qty
        if product_id in self.cart:
            self.cart[product_id]['qty'] = product_quantity
        self.session.modified = True
```


{% endraw %}
