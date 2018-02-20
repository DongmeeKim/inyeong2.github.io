---
layout: post
title: 패캠 장고 13. Onlineshop_ App 생성 및 admin 설정
categories: Django
tags: [Django, python, 장고]
comments: true
---

패스트캠퍼스_ Python & Django를 활용한 웹 서비스 개발 CAMP 내용 정리

### Onlineshop_App생성 및 admin 설정

#### 가상환경안에서 onlineshop 프로젝트 생성
Dstagram때 처럼 cmd창에서 myenv가상환경 만들고, mysite까지 생성하고 pycharm에서 프로젝트 열기
(강의때 pycharm에서 바로 가상환경내에서 여는방법이 있었는데 나는 잘 안된다ㅠㅠ)

#### App 생성
콘솔에 `python manage.py startapp [app이름]` 을 입력하여 shop, orders, cart App을 각각 생성해준다.

#### settings.py 에 shop app 추가
`settings.py > installed_apps`에 'shop', 추가 
(이후에 migrate 해주기)

#### (shop) 모델 만들기
shop App의 모델을 만들기 전, 장고에서 이미지와 관련된 작업을 위해 사용하는 Pillow라는 라이브러리 설치 `pip install pillow`
models.py에 Category와 Product Class 만들기
이후에 makemigrations 와 migrate를 수행

#### (shop) admin 생성 및 계정만들기
shop/admins.py에 Category와 Product 모델을 import해옴. 각각 CategoryAdmin과 ProductAdmin의 옵션모델도 생성.
완성된 admin페이지
![admin_shop](https://user-images.githubusercontent.com/34964514/35193302-e398c0f6-fee3-11e7-8a53-26d3216ee8b4.JPG)

