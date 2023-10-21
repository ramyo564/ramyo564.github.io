---
layout: single
title: "[Skill Interview] XML, JSON, YAML 형식 내용 정리 및 비교 분석"
categories: Skill_Interview
tags:
  - 기술면접
toc: true
toc_sticky: true
author_profile: false
sidebar:
---
# XML, JSON, YAML 정확하게 알아보자

- 기술면접에서 api 질문 json 질문 그리고 xml,json,yaml이 뭔지 그리고 각각 왜 쓰는지에 대한 꼬리 질문을 받았는데 사실 그냥 별생각 없이 사용해서 대답을 못 했다.
- 처음에 JSON 이 뭐냐고 질문받았을 때부터 당황했는데 왜냐하면 JSON이 JSON인데 이게 뭐냐고 한다면....그러게 이게 뭐지? 라는 게 내가 생각한 첫 생각이었다.결론은 모른다는 소리!
- 면접 덕분에 막연하게 알고 있다고 생각했지만 사실상 모르는 부분들이 많이 확인되었는데 부족한 부분을 채워보자 👍

JSON, YAML, XML은 모두 데이터를 표현하는 방식이다.     
형태와 문법이 조금씩 다른 점이 있는데 이 차이점에 대해 정확히 설명할 수 있다면 굳
## XML

자바에서 xml을 써본 기억만 있었지 이걸 또 설명하려니까 말을 못 했었다.     
내가 기억하는 부분은 그냥 메이븐으로 설정 적용했던 게 끝이였으니..ㅜ
그때 당시 또 설정 파일은 더 이상 메이븐 잘 안 쓰고 그래들을 쓴다고 해서 대충 넘겼었다.    
아래와 같이 xml은 html 처럼 태그형식으로 사용된다.

```
<?xml version="1.0" encoding="UTF-8"?>
<users>
  <user>
    <name>홍길동</name>
    <score>95</score>
    <hobby>
      <element>Soccer</element>
      <element>Ninza</element>
    </hobby>
  </user>
  <user>
    <name>이순신</name>
    <score>100</score>
    <hobby>
      <element>Sing</element>
      <element>Dancing</element>
    </hobby>
  </user>
</users>
```

 **XML 유효성 검증 사이트:** https://www.xmlvalidation.com/

**※ XML 선언 ※**

  XML 문서는 기본적으로 `<xml>` 태그를 이용해서 XML 문서임을 명시할 수 있다. 속성으로는 다음과 같은 것들을 넣을 수 있다.

 **- version**: XML 문서의 버전을 명시한다.
 **- encoding**: XML 문서의 문자 셋(Character Set)을 명시한다. 일반적으로 UTF-8을 사용한다.
 **- standalone**: XML 문서 외부 소스 데이터에 의존하는지의 여부를 명시한다.

따라서 일반적으로 다음과 같이 작성할 수 있다.

```
<?xml version="1.0" encoding="UTF-8"?>
```

**※ XML 문법 ※**

  XML 문서는 규칙적인 형태를 가지고 있다. 가장 중요한 문법으로는 XML 요소들은 시작 태그와 종료 태그로 구성된다는 점이다. 태그는 꺽쇠(<>)를 이용해 명시하며, 닫는 태그에는 슬래시(/)를 함께 넣어준다. 예를 들어 TITLE이라는 이름의 태그를 사용한다고 하면 다음과 같이 사용할 수 있다.

```
  <TITLE>내용</TITLE>
```

  이 때 시작태그와 종료 태그는 대소문자까지 모두 동일해야 한다. 또한 태그 안에 속성을 명시할 때는 따옴표를 넣어주어야 한다. 속성을 사용할 때는 다음과 같이 사용할 수 있다.
  
```
  <TITLE color="red" type="bold">내용</TITLE>
```


**※ XML 주석 ※**

  
  XML은 주석(Comment)을 넣을 수 있다. XML의 주석은 

```
<!-- 주석 내용 --> 
```

  형태로 사용할 수 있다.


## JSON

JSON도 XML과 비슷하게 데이터를 처리하기 위한 형식이다.     
일반적으로 서버와의 통신 규약인 REST API 를 사용할 때 가장 많이 사용되어 최근에 XML 보다  JSON 형식이 채택되고 있다고 한다.      
사실상 모든 프로그래밍 언어에서 JSON을 지원한다는 점에서 XML 과 YAML에 비해서 채택률이 높다     
하지만 JSON은 주석을 사용할 수 없다는 특징이 있다.       

```
{

	"users": {
	
		"1": {
		
			"name": "홍길동",
			
			"score": 95,
			
			"hobby": ["Soccer", "Ninza"]
		
		},
		
		"2": {
		
			"name": "이순신",
			
			"score": 100,
			
			"hobby": ["Sing", "Dancing"]
		
		},
		
		"3": {
		
			"name": "ㅋㅋㅋ",
			
			"score": 97,
			
			"hobby": ["Coding", "Hiding"]
			
		}
	
	}

}
```


기본적으로 JSON 형식에서는 키(Key) 값이 서로 다른 형태로 사용된다. 그래서 위와 같이 사용자를 1, 2, 3으로 구분하는 방식으로, 사용자 명단에 대한 내용을 작성할 수 있다. 다만 JSON은 XML과 다르게 꺽쇠가 사용되지 않고 대괄호({})와 큰 따옴표를 이용해 계층형 구조를 형성한다는 특징이 있다     

## YAML

**YAML** 또한 JSON과 비슷하게 사람이 읽기 쉬운 형태의 데이터 표현 형식이다. YAML은 XML과 문법적으로 유사한 점이 많다. YAML에서도 주석을 사용 가능하며 개행, 공백으로 블록을 인식한다. 다만, 태그를 사용하지 않고 공백 위주로 데이터를 구분하므로 한 줄로 작성할 수 없다는 특징이 있다.      

그리고 JSON은 한글 등의 멀티 바이트 문자를 인코딩하여 보여주지만 **YAML**은 한글과 같은 유니코드를 그대로 사용할 수 있다는 장점이 있다. 일반적으로 API를 만들 때에는 JSON을 사용하며, YAML은 설정 파일을 작성할 때 가장 많이 사용된다는 특징이 있다. 심지어 YAML는 상속(Inherit) 등의 기능도 적용할 수 있다는 점에서 효율적이다.

```
users:
  1:
    name: 홍길동
    score: 95
    hobby:
      - Soccer
      - Ninza

  2:
    name: 이순신
    score: 100
    hobby:
      - Sing
      - Dancing

  3:
    name: ㅋㅋㅋ
    score: 97
    hobby:
      - Coding
      - Hiding
```


[출처](https://ndb796.tistory.com/251)