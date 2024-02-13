---
layout: single
title: " [Django] Cart summary template optimization "
categories: Django
tags:
  - Python
  - Cart
  - functionality
  - Cart
  - summary
  - template
  - optimization
  - 얕은복사
  - 깊은복사
toc: true
toc_sticky: true
author_profile: false
sidebar:
---
# Cart (6)

/ Optimization / 얕은복사 / 깊은복사

## 최적화
- 얕은복사 -> 깊은복사

### Cart.py
```python
class Cart():
    def __iter__(self):
        all_product_id = self.cart.keys()
        products = Product.objects.filter(id__in=all_product_id)
        import copy
        cart = copy.deepcopy(self.cart)
        for product in products:
            cart[str(product.id)]['product'] = product
        for item in cart.values():
            item['price'] = int(item['price'])
            item['total'] = item['price'] * item['qty']
            yield item
```
>- 완전복제를 위해서 `cart = self.cart.copy()` 가 아닌 copy.deepcopy 를 이용해서 객체를 복사해준다.
>- 참고 : https://wikidocs.net/16038

