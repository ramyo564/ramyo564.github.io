---
layout: single
title: "[Basic Java] Hash Table ( Hash Map )"
categories: Basic_Java
tags:
  - Java
toc: true
toc_sticky: true
author_profile: false
sidebar:
---
# Hash Table이란?

예를 들어 성적에 대한 점수를 Array 자료구조 관리할 때 성적을 조회하려면 어떤 과목에 대한 성적을 알기 위해서는 O(n) 의 시간복잡도를 갖게된다.    
하지만 이 때 어떤 값이 어떤 과목인지 명확하게 알기 어렵다.    

```java
sub = [100, 98, 22, 44]
```

이런 상황에서 Hash table 은 `{ "eng" : 100 }` 형태로 데이터를 저장하고 검색할 수 있다.    
이때 O(1) 의 식간 복잡도로 key값인 eng value 값 100 으로 직접 조회할 수 있다.   

컴픂터 과학에서 가장 중요한 **검색/삽입/삭제 작업에 효율적인 자료구조**중 하나로 평균 O(1) 의 시간복잡도로 **Key-Value 형태의 값을 저장하고 검색**할 수 있다.  

```java

sub = {
	"eng" : 100,
	"math" : 98,
	"kor" : 22,
}
```

- Key 값은 고유해야하며 만일 중복된 key가 있으면 먼저 있던 key와 value를 대체한다.
- 저장 순서를 보장하지 않으므로 순서에 의존적인 작업에는 적합하지 않을 수 있다.

## Hash Table의 동작 원리

![](https://i.imgur.com/54f74ka.png)

Hash Table에 데이터를 추가하거나 조회할 때, Hash Function을 사용한다. Hash Table에서 해시 함수는 입력된 Key를 table의 크기에 적합한 인덱스 값으로 변환하는 것이 목적이다. 따라서 출력의 길이(인덱스 범위)는 해시 테이블의 크기에 의존적이다. 예를 들어, 크기가 10인 해시 테이블에서 사용하는 해시 함수의 출력 범위는 0부터 9까지다.     


### Hash Table의 크기를 무엇을 기준으로 할 까?

”Hash Table의 크기는 2의 멱수 (2^0, 2^1, 2^2, , 2^{10}, 2^{11})에 가깝지 않은 소수(prime number)”를 선택하는 것이 좋다. 해시 테이블의 크기가 2의 거듭제곱인 경우, 모듈러 연산의 결과가 일부 패턴을 보이는 경우가 있다. 이러한 패턴은 table 일부분에만 데이터가 집중적으로 저장되는 현상을 초래하고, 따라서 충돌이 더 자주 발생할 수 있다. Hash Table의 크기를 2의 멱수 (2^0, 2^1, 2^2, , 2^{10}, 2^{11})에 가깝지 않은 소수(prime number)로 선택하면, 소수는 자연스럽게 2의 거듭제곱과는 다른 특성을 가지므로, 모듈러 연산 결과가 더 다양하게 나온다. 이로 인해 같은 해시 코드를 생성할 확률은 직접적으로 줄어들지 않지만, 데이터가 더 고르게 분포됨으로써 효율적인 해시 테이블 작동을 도와준다.     
[참고 ](https://planetmath.org/goodhashtableprimes)

해시 테이블에서 사용하는 해시 함수가 해시 코드를 계산하는 방법은 간단한 방식부터 복잡하고 최적화된 방식까지 다양하다.    
여기서 각 문자열의 문자들은 ASCII 값을 합하여 해시코드를 만드는 Hash 함수로 설명할 수 있다.   

![](https://i.imgur.com/pVWdv7P.png)

각 문자열의 문자들의 ASCII 값을 합하여 그 합을 해시 코드로 사용하면 다음과 같다.

```jsx
강(44053) + 호(54812) + 동(46020) = 144885
유(50976) + 재(51116) + 석(49457) = 151549
이(51060) + 수(49688) + 근(44396) = 145144
민(48148) + 경(44221) + 훈(54801) = 147170
```

위 그림과 같이 크기가 7인 table에 값을 저장하기 위해서는 각 해시 코드를 7로 나눈 나머지 값을 사용하여 인덱스를 결정할 수 있다. 이 방법을 "모듈로 해싱(Modulo Hashing)"이라고 부른다.

```jsx
강호동 : 144885 (해시 코드) % 7 = 4 (해시 테이블의 인덱스)
유재석 : 151549 (해시 코드) % 7 = 3 (해시 테이블의 인덱스)
이수근 : 145144 (해시 코드) % 7 = 6 (해시 테이블의 인덱스)
민경훈 : 147170 (해시 코드) % 7 = 5 (해시 테이블의 인덱스)
```

- Hash Table은 해시 알고리즘을 이용해서 Key 값을 기준으로 테이블에 저장할 위치를 $O(1)$ 시간에 계산한다. Key 값으로 가져올 때도 같은 동작원리로 $O(1)$ 시간에 데이터를 조회할 수 있다.   

![](https://i.imgur.com/UhW8Yvc.png)

- 하지만 두 개 이상의 키가 동일한 해시 값을 가질 경우 충돌이 발생한다. 충돌을 과니하는 많은 방법을이 있지만 충돌이 빈번하게 발생할 경우 성능 저하를 가져올 수 있다. 

## 해시 테이블에서 충돌이 발생할 경우는?

- 해시 테이블 내에 어떤 키가 이미 자리 잡고 있는 상태에서 다른 키가 삽입을 시도하는 경우에 해시함수가 서로 다른 입력에 대해 동일한 값을 반호나하는 경우 Hash Collision이 발생한다.

![](https://i.imgur.com/FZ6ux9m.png)

-  해시 함수는 임의의 크기의 입력을 고정 크기의 해시 값으로 변환하는데 이 때 해시 함수의 출력 공간이 입력 공간보다 작을 수 있기 때문에 해시 충돌이 발생할 수 있다.
- 예를 들어 입ㄹ력 값이 문자열이고 값이 저장되는 배열의 index 값의 정수일 때, 문자열의 값은 무한히 큰데 반해 정수 값의 범위는 table의 크기라서 상대적으로 작게 고정되어있기 때문이다.

### 해결방법 

#### 1. Separate Chaning

- 같은 주소로 해싱되어 충돌이 일어나는 Entry 를 링크드 리스트로 연결해서 저장하는 방식으로 해시 충돌을 피할 수 있다.

![](https://i.imgur.com/H4C9lZp.png)

- Chaning 방법에서 입의의 키에 해당하는 엔트리를 저장할 때는 해시 값이 가리키는 bucket의 링크드 리스트에 삽입한다.
- 이 때 내가 사용하고 있는 링크드 리스트의 구현체에 따라 맨 앞 혹은 맨 끝에 삽입하는 게 좋다. O(1)

#### 2. Open Addressing

- Separate Chaning 방식과 달리 추가 공간을 사용하지 않고 충돌을 해결하는 방법이다.

![](https://i.imgur.com/nYi1DkK.png)

- 해시 충돌이 일어나도 정해진 hash table 공간에 다른 위치에 저장한다. 이러한 특징 때문에 테이터가 key의 해시 결과 반환되는 bucket의 인덱스와 일치하는 주소에 저장된다는 보장은 없다.
- 개방 주소법 (Open Addressing)에서 엔틀리 (Key-value pair) 를 해시 테이블에 삽입하는 과정
	- 해시(key) -> 해시 충돌 발생 X -> 해시 결과 반환된 bucket 위치에 저장
	- 해시(key) -> 해시 충돌 발생 O -> 정해진 규칙에 의해 다음 자리를 찾아 저장
- 충돌이 일어났을 때, 다음 주소를 저장하는 방법으로는 대표적으로 세가지가 있다.
	- 선형 탐색 (Linear Probing)
		- Open addressing 방식 (1) - Linear probing
		- Linear Probing 방식의 경우 특정 영역에서 키가 몰리면 탐색 횟수가 느어나 성능이 떨어진다.
		- 이차원 탐색 (Quadratic Probing)과 더블 해싱(Double Hashing) 으로 충돌을 피하면서, 군집 문제를 해결할 수 있음
		- ![](https://i.imgur.com/5348bBX.png)
	- 이차원 탐색 (Quadratic Probing)
	- 더블 해싱 (Double Hashing)
- 개발 주소 방식에서 주의사항
	- 데이터가 key의 해시 결과를 반환하는 bucket의 인덱스와 일치하는 주소에 저장된다는 보장은 없기 때문에 중간에 데이터를 삭제할 때, 삭제된 데이터에 자리가 비어지게 되면 이후에 데이터를 검ㅁ색할 때 문제가 발생한다.


![](https://i.imgur.com/YJCbXe9.png)

## Load Factor는 무엇일까? 성능에 어떤 영향을 미칠까?

**Load Factor**는 해시 테이블에 저장된 항목의 수를 해시 테이블의 전체 크기로 나눈 값으로, **해시 테이블의 "가득 찬 정도"를** 나타낸다. 예를 들어, 로드 팩터가 0.5라면 해시 테이블의 절반만 항목으로 채워져 있다는 것을 의미한다.

Separate Chaining과 Open Addressing 모두에서 Load Factor는 중요한 역할을 한다. 그러나 각 방식에서 Load Factor의 의미와 그에 따른 영향이 약간 다르다.

![](https://i.imgur.com/XDHn0vN.png)

1. **Separate Chaining**:
    - **Load Factor가 증가하면 각 버킷에 저장된 연결 리스트의 평균 길이가 길어진다.**
    - **검색 성능은 load factor에 비례해서 저하될 수 있다.**
    - 이로 인해 충돌 시 처리하는 데 필요한 시간이 증가할 수 있다.
    - 따라서, Load Factor가 특정 임계값 (예: 0.75)을 초과하면 해시 테이블의 크기를 늘려서 리사이징하는 것이 일반적이다.
2. **Open Addressing**:
    - Load Factor가 증가하면 해시 **테이블 내의 사용 가능한 빈 슬롯이 줄어들게 된다. 이로 인해 새로운 항목을 삽입하거나 기존 항목을 찾을 때 발생하는 충돌의 수가 증가**하게 되며, 이는 성능 저하로 이어질 수 있다.
        
    - 검색 성능은 load factor가 1에 가까워 질 수록 급소가게 커진다.
        
        ![](https://i.imgur.com/VfkI5jJ.png)
        
    - Open Addressing에서는 Load Factor가 너무 높아지면 성능이 크게 저하될 수 있으므로, 일반적으로 Load Factor의 임계값을 Separate Chaining보다 낮게 설정한다 (예: 0.5 또는 0.6).
        

결론적으로, 두 해시 테이블 구현 방식 모두에서 Load Factor는 중요한 역할을 한다.

## 해시 테이블에서 동적 리사이징은 뭐고 언제 필요할까?

동적 리사이징은 해시 테이블의 크기를 동적으로 조정하는 과정입니다. 초기에 할당된 배열의 크기가 고정되어 있기 때문에, 데이터의 양이 증가하면서 배열의 크기를 초과할 가능성이 있습니다. 이럴 때, 해시 테이블의 크기를 증가시켜 (보통 두 배로) 새로운, 더 큰 배열에 기존의 데이터를 재해시하여 저장하는 과정을 동적 리사이징이라고 합니다.

동적 리사이징은 주로 해시 테이블의 로드 팩터가 특정 임계값 (예: 0.75)을 초과할 때 발생합니다. 로드 팩터는 해시 테이블에 저장된 항목의 수를 해시 테이블의 전체 크기로 나눈 값입니다. 로드 팩터가 높아지면 충돌의 확률이 증가하므로, 동적 리사이징을 통해 이를 해결하려고 합니다.

동적 리사이징을 다음과 같이 구현해 볼 수 있습니다.

- Entry Class
    
    ```java
    class Entry<K, V> {
        K key;
        V value;
    
        Entry(K key, V value) {
            this.key = key;
            this.value = value;
        }
    }
    ```
    
- Hash Table Class
    
    - `LOAD_FACTOR & resize()`
        
        ```java
        import java.util.LinkedList; // Doubly Linked List
        
        public class ChainingHashTable<K, V> {
        		**private static final float LOAD_FACTOR = 0.75f;**
        
            private LinkedList<Entry<K, V>>[] table; //buckets
            private int numberOfItems;
        
            public ChainingHashTable(int capacity) {
                table = new LinkedList[capacity];
                numberOfItems = 0;
        
                for (int i = 0; i < capacity; i++) {
                    table[i] = new LinkedList<>();
                }
            }
        
        		private void resize() {
        				LinkedList<Entry<K, V>>[] originalTable = table;
        
        				// Create new table
                newTable = new LinkedList[table.length * 2];
        
                for (int i = 0; i < table.length * 2; i++) {
                    newTable[i] = new LinkedList<>();
                }
        
        				for (LinkedList<Entry<K, V>> bucket : originalTable) { // 세로
                    for (Entry<K, V> entry : bucket) {                 // 가로
                        put(entry.key, entry.value);
                    }
                }
        
        				this.table = newTable;
                this.numberOfItems = 0;
        
            }
        
        		private boolean isResizingRequired() {
        		    return (float) size / table.length > LOAD_FACTOR;
        		}
        }
        ```
        
    - **`put()`**
        
        ```java
        public void put(K key, V value) {
            int indexOfBucket = this.hash((String) key);
            LinkedList<Entry<K, V>> bucket = table[indexOfBucket];
        
        		for (Entry<K, V> entry : bucket) {
        	      if (entry.key.equals(key)) {
        	          entry.value = value;
        	              return;
                    }
                }
        		}
        
            bucket.add(0, new Entry<>(key, value));
            numberOfItems++;
        
        		if (isResizingRequired()) resize();
        }
        ```