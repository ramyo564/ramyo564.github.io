---

layout: single
title: " [Django] [Django] Cart functionality Add Summary info "
categories: Django
tag: [Python,"[BIG][Django] Cart functionality","[Django] Cart Add info template"]
toc: true
toc_sticky: true
author_profile: false
sidebar:

---
# Cart(4)
{% raw %}
/ 장바구니 정보 페이지 /

## Update the cart summary template

### cart-summary.html
```python
{% for item in cart %}
{% with product=item.product %}


{% endwith %}
{% endfor %}

```

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



{% endraw %}
