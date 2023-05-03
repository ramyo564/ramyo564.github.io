---

layout: single
title: " [Django] Project setup "
categories: Django
tag: [Python,"[BIG][Django] Django4 project setup","파이썬 가상환경"]
toc: true
toc_sticky: true
author_profile: false
sidebar:

---
# Setup

/ 파이썬 가상환경 설정 /

- 이제 슬슬 포폴도 만들겸 이전에 만든 프로젝트들을 정리하는데 막상 까보니 기억이 잘 안남... 노션에 메모해 두었지만 대충 정리해서 못 알아보겠...
- 다시 복습겸 정리하면서 처음부터 만들 생각..
- 너무 간단하게 정리하면 나중에 못 알아보고 너무 자세하게 정리하면 시간이 오래걸려서 속도가 느려지고... ㅜ
>- 그래도 어차피 한 번 만들어본거라 금방 다시 만들 수 있을 거라 생각함

## 파이썬 가상환경 설정

### 가상환경이 없을 경우
```cmd
pip install virtualenv
```

### 깃헙으로 관리하는 해당 프로젝트 폴더
```cmd
프로젝트폴더 / virtualenv venv
```

>- vscode 에서 만들어도 되지만 깃헙에서 클론한 폴더에 cmd로 가상환경을 만들면 라이브러리는 알아서 ignore 에 들어가서 아주 좋다.
>- venv 는 가상환경 폴더 이름
>- vscode 에서 가상환경을 선택해주면 터미널에서 확인이 가능하다.
>	- ![](https://i.imgur.com/DxmWEKJ.png)

>- pip list 를 통해 리스트가 비어 있다면 현재 가상환경으로 돌아가는중인거다.
>	- ![](https://i.imgur.com/qyqLX5M.png)

>- 가상환경을 이용해야되는 이유는 우선 라이브러리 버전이 안 맞아서 꼬이면 여려모로 머리를 감싸는 상황이 연출된다.
>- 버전 관리를 위해 가상환경을 사용하는게 좋다.
>	- (무엇보다 지저분해서 보기 안 좋다...)

## 장고 설치 및 app 설치
```cmd
pip install django

django-admin startproject ecommerce
```

- 이제 ecommerce 폴더에서 python manage.py runserver 를 입력해주면 끝
- ![](https://i.imgur.com/5zQmBCt.png)
- 이전에 만들었을 때는 4.0이였는데 4.2로 업글되었나 보다.
	- 참고 : https://docs.djangoproject.com/en/4.2/releases/4.2/

