---
layout: single
title: "[Basic Python] pass by assignment"
categories: Basic_Python
tags:
  - Python
  - PassByAssignment
toc: true
toc_sticky: true
author_profile: false
sidebar:
---
# Pass by assignment

"파이썬에서 pass by assignment"은 파이썬에서 변수와 객체 간의 상호 작용 방식을 설명하는 개념 중 하나다. 이것은 함수 호출 및 변수 할당과 관련이 있다.      

파이썬에서 변수는 실제 데이터를 직접 저장하는 것이 아니라 객체에 대한 참조(reference)를 저장한다. 이때 "pass by assignment"라는 용어가 사용된다. 이것은 다음과 같이 동작한다:     

1. 객체 생성: 데이터나 값이 저장된 객체가 생성.
2. 변수 할당: 변수는 해당 객체에 대한 참조를 할당한다. 이것은 변수가 실제로 객체를 보유하는 것이 아니라 객체를 가리키는 포인터 역할을 한다.
3. 함수 호출: 함수에 인수로 변수를 전달할 때, 해당 변수가 가리키는 객체의 참조가 함수로 전달된다. 함수 내에서 변수의 값을 변경하면 해당 변수가 가리키는 객체의 상태가 변경되는 것이 아니라, 변수가 다른 객체를 가리키도록 변경된다.


```python
def modify_list(my_list):
    my_list.append(4)
    my_list = [1, 2, 3]  # 새로운 리스트를 가리키도록 변수를 변경

my_var = [0]
modify_list(my_var)
print(my_var)  # 출력: [0, 4]

```

이 예제에서 `my_var`는 초기에 `[0]` 리스트를 가리키고 있다. 그러나 `modify_list` 함수 내에서 먼저 `append` 메서드를 사용하여 원래 리스트를 변경한 다음, 새로운 리스트 `[1, 2, 3]`를 가리키도록 `my_list` 변수를 변경한다. 그 결과, `my_var`는 여전히 `[0, 4]`를 가리키고 있다.      

이것이 "pass by assignment"라고 불리는 이유는 변수가 객체에 대한 참조를 가리키는 포인터(할당)를 전달하기 때문다. 변수와 객체 사이의 상호 작용은 참조에 의해 이루어지며, 함수 내에서 변수에 새로운 객체를 할당하더라도 원래 객체는 변경되지 않는다.