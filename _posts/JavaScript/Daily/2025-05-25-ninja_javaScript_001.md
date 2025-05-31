---
layout: single
title: "[Basic JavaScript] 객체에 대한 메모리 주소 with Redux 잘 사용하기"
categories: Basic_Java
tags:
  - Java
toc: true
toc_sticky: true
author_profile: false
sidebar:
---
# 📘 Primitive 값 vs Reference 값

JavaScript에서 값을 다룰 때 **기본형(Primitive)**과 **참조형(Reference)** 두 가지 방식이 있습니다. 이 차이를 이해하는 것은 **버그를 피하고 올바른 상태 업데이트 로직을 짜기 위해 매우 중요**합니다.

---

## ✅ Primitive Values (기본형 값)

### 예시: `string`, `number`, `boolean`, `null`, `undefined`, `symbol`, `bigint`

```javascript
let name = "Max";
let newName = name;

newName = "Manu";
console.log(name); // "Max"
```

- `name`은 `"Max"`라는 **문자열(string)** 값을 가지고 있고, `newName`에 복사됨.
- 문자열은 **기본형**이기 때문에 값 자체가 복사됨 (복사 = 독립적인 값이 됨).
- `newName`을 바꿔도 `name`은 영향 없음.

### ✔ 핵심: **Primitive 값은 복사될 때 값 자체가 복사된다.**

---

## 📦 Reference Values (참조형 값)

### 예시: `object`, `array`, `function`

```javascript
let person = { name: "Max" };
let anotherPerson = person;

anotherPerson.name = "Manu";
console.log(person.name); // "Manu"
```

- `person`은 객체이고, `anotherPerson`은 같은 객체를 **참조**함.
- 즉, 두 변수는 **같은 메모리 주소**를 가리킴.
- 하나를 바꾸면 다른 하나도 영향을 받음.
### ✔ 핵심: **Reference 값은 복사될 때 참조(주소)가 복사된다.**

---

## 📎 객체 복사 (얕은 복사 vs 깊은 복사)

### 얕은 복사 (Shallow Copy)

```javascript
let person = { name: "Max" };
let copiedPerson = { ...person }; // spread 문법

copiedPerson.name = "Manu";
console.log(person.name); // "Max"
```

- `...`을 사용하면 객체의 **1단계 속성만 복사**됨.
- 중첩된 객체나 배열은 여전히 참조됨.

### 깊은 복사 (Deep Copy)

```javascript
let person = { name: "Max", address: { city: "Seoul" } };
let copiedPerson = JSON.parse(JSON.stringify(person));

copiedPerson.address.city = "Busan";
console.log(person.address.city); // "Seoul"
```

- `JSON.parse(JSON.stringify(...))`을 사용하면 깊은 복사가 가능하지만,
    - 함수, `undefined`, `Date`, `Map`, `Set` 등은 손실될 수 있음.
- 복잡한 경우에는 `lodash.cloneDeep()` 같은 라이브러리 사용 추천.

### 💡 요약

|구분|복사 시|변경 시 영향|
|---|---|---|
|기본형 (Primitive)|값 자체 복사|서로 독립적|
|참조형 (Reference)|메모리 주소 복사|서로 영향 줌|
|얕은 복사|1단계만 복사|중첩 데이터는 공유|
|깊은 복사|모든 계층 복사|완전히 독립|

## 🧊 불변 업데이트 패턴 (Immutable Update Patterns)

Redux에서는 상태(state)를 직접 변경해서는 안 되며, 항상 **불변성(immutability)**을 유지해야 합니다. 즉, 기존 상태 객체를 수정하지 말고, **복사본을 만든 다음 필요한 변경을 적용해야** 합니다.

### 왜 불변성이 중요한가요?

- 변경 사항을 추적하기 쉽고, 변경이 일어난 지점을 명확히 알 수 있음.
- React의 `shouldComponentUpdate`, `React.memo`, `useSelector` 같은 최적화 기능이 제대로 작동함.
- 상태의 이전 버전을 보존하거나 "되돌리기/앞으로가기" 기능 구현이 쉬워짐.

---

## ✅ 불변하게 업데이트하는 일반적인 패턴들

### 1. 객체(Object)의 불변 업데이트

```javascript
return {
  ...state,
  property: newValue
}
```

예시:

```javascript
const nextState = {
  ...state,
  user: {
    ...state.user,
    name: "Alice"
  }
}
```

### 2. 배열(Array)의 불변 업데이트

#### 항목 추가하기

```javascript
return [...state.items, newItem];
```

#### 항목 제거하기

```javascript
return state.items.filter(item => item.id !== targetId);
```

#### 항목 수정하기

```javascript
return state.items.map(item =>
  item.id === targetId ? { ...item, value: newValue } : item
);
```


## 🛠️ 복잡한 업데이트 처리 팁

중첩 구조가 깊거나, 복잡한 로직이 있을 경우에는 다음을 고려하세요:

- `immer` 라이브러리를 사용하면 불변 업데이트를 간단히 작성할 수 있음.
    - 예: `createSlice` (Redux Toolkit 사용 시 자동으로 immer 내장)
- 헬퍼 함수로 중복 코드를 분리하여 관리

---

## 🔁 예제 코드 (객체 내 배열 업데이트)

```javascript
function postsReducer(state = initialState, action) {
  switch (action.type) {
    case 'postUpdated': {
      const updatedPosts = state.posts.map(post =>
        post.id === action.payload.id
          ? { ...post, content: action.payload.content }
          : post
      );
      return {
        ...state,
        posts: updatedPosts
      };
    }
    default:
      return state;
  }
}
```

## 🔥출처

- [Reference vs Primitive Values - Academind](https://academind.com/tutorials/reference-vs-primitive-values) 
- [Immutable Update Patterns - Redux](https://redux.js.org/usage/structuring-reducers/immutable-update-patterns)