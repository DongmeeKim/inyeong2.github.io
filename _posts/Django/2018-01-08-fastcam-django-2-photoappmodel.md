---
layout: post
title: 패캠 장고 4. Dstagram_photo app 생성 및 모델 만들기
categories: Django
tags: [Django, python, 장고]
comments: true
---

패스트캠퍼스_ Python & Django를 활용한 웹 서비스 개발 CAMP 내용 정리

### Dstagram photo app 생성 및 모델 만들기

#### photo app 생성
가상환경(myenv)내의 Dstagram 프로젝트 폴더에 하나 이상의 Django App으로 구성되어 있음. 여기선 photo app을 먼저 생성함 
( 규모가 큰 Django Project는 보통 여러 개의 Django App들을 모듈화하여 구성함. 잘 모듈화된 App들로 구성하면 개발 및 유지 보수가 효율적이고 여러 웹 프로젝트에서 쉽게 재사용이 가능함.)

1. photo app을 새롭게 생성하기 위해서 Pycharm 터미널에 아래와 같은 명렁어 입력
```
python manage.py startapp photo
```
2. `mysite/settings.py` 의 INSTALLED_APPS 리스트에 APP명(photo) 추가
3. photo app의 DB 초기화를 위한 명령어
(데이터베이스에 테이블을 만드는 과정 / migrate명령은 INSTALLED_APPS에 등록된 어플리케이션에 한하여 실행됨)
```
python manage.py migrate
```

4. 서버 실행
```
python manage.py runserver
```
크롬에 `localhost:8000` 입력하면 app이 잘 생성된 것을 확인 가능
(만약 8000포트가 사용 중이여서 다른 포트번호를 지정하고 싶다면,```pyhton manage.py runserver 8080``` )

#### 객체와 모델
(참고 : [장고걸스 튜토리얼](https://tutorial.djangogirls.org/ko/django_models/))
* 객체(object)?
: 객체 = 속성과 행동을 모아놓은 것
속성은 객체 속성(properties), 행동은 메서드(methods)로 구현됨
작성자, 내용, 업로드일, 수정일같은 것은 속성

```
Post(사진포스팅)
ㅡㅡㅡㅡ
author(작성자)
text(내용)
created_date(업로드일)
updated_date(수정일)
```
* 장고 모델?
: 우리가 장고 모델을 만들어 이 모델을 저장하면 그 내용이 데이터베이스에 저장됨.(데이터베이스에 유저들이 올린 사진, 글등이 저장되어 있음) 장고에서는 데이터베이스로 `SQLite`를 사용함.
(하나의 모델은 하나의 테이블)  **데이터베이스 안의 모델 = 테이블**

#### 사진 포스팅을 저장하는 모델 만들기 

photo/models.py에 아래와 같은 코드 입력
```
from django.db import models
from django.contrib.auth.models import User

class Photo(models.Model):
    author = models.ForeignKey(User, related_name='photo_posts')
    text = models.TextField()
    created = models.DateTimeField(auto_now_add=True)
    updated = models.DateTimeField(auto_now=True)

    class Meta:
        ordering = ('-updated',)
    def __str__(self):
        return self.author.username + " " + self.created.strftime("%Y-%m-%d %H:%M:%S")
```
(field, class,ForeignKey 등 위 코드에 대한 설명 추가)

#### 데이터베이스에 모델을 위한 테이블 만들기
데이터베이스에 models.py에서 생성한 `Photo` 모델을 추가해야함.
1. 장고 모델에 몇 가지 변화가 생겼다는 것을 알게 해주기 위해 makemigrations를 해줌(변경사항에 대한 migration file을 생성함)
```
python manage.py makemigrations photo
```
2. 변경 사항을 데이터베이스에 적용
```
python manage.py migrate photo
```

(참고) sqlmigrate 명령은 sql명령어를 보여줌

---
## Reference
[장고걸스 튜토리얼](https://tutorial.djangogirls.org/ko/django_models/)
[장고 튜토리얼 공식문서](https://docs.djangoproject.com/ko/2.0/intro/tutorial02/)

