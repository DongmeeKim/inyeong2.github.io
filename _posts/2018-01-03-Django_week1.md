---
layout: post
title: [패캠_Django] 2주차.(실습) 장고 설치 및 프로젝트생성 ~ 템플릿작성
---

패스트캠퍼스_ Python & Django를 활용한 웹 서비스 개발 CAMP 실습내용 정리
(배운내용을 좀 더 확실하게 복습하는 겸 정리해놓으면 좋을 것 같다!!)



##### 장고 설치 및 프로젝트 생성
(윈도우에서 cmd 실행)
```
> cd [프로젝트파일 만들 위치]
> pip install django==1.11
> django-admin startproject [프로젝트명]
```
(==django-admin== 은 스크립트로 디렉토리와 파일들을 생성함)
위의 명령을 입력해주면 지정해준 위치에서 프로젝트명을 가진 디렉토리에 가면
(디렉토리 구조 확인 cmd 명령: >tree [디렉토리명] /F)
![django1](https://user-images.githubusercontent.com/34964514/34638592-1d48809e-f312-11e7-8a1d-88c1ea9434c5.jpg)
위와 같이 manage.py와 디렉토리안에 상위 디렉토리와 같은 네임으로 하나의 디렉토리가 더 생겼고 그 안에 4개의 파이썬파일 생성되어있음.

==manage.py== : 우리가 장고와 커뮤니케이션(상호작용)할 수 있는 것 (Django 프로젝트와 다양한 방법으로 상호작용하는 커맨드라인의 유틸리티)

==_ _init_ _.py== : 생성 시 자동으로 생성되는 파일

==settings.py ==: 현재 프로젝트의 여러가지 환경설정을 함

==urls.py== : 현재 프로젝트의 url 선언을 저장함 (Django로 작성된 사이트의 '목차'라고 생각)

==wsgi.py== : 현재 프로젝트를 서비스 하기 위해 웹과 커뮤니케이션할 수 있는 웹 서버의 진입점(세관같은 역할)

##### APP 추가
프로젝트를 생성해줬으니 이제 우리가 추가하고 싶은 기능을 가진 app을 생성 (Pycharm 터미널 이용)
```
>python manage.py startapp firstapp
```
'firstapp'이라는 앱을 추가해주고 나면, 
![django2](https://user-images.githubusercontent.com/34964514/34638741-55ffb45e-f315-11e7-8ae1-07fc701079a0.JPG)
위와 같이 firstapp디렉토리 안에 6개의 파일과 migrations 디렉토리가 생성됨(_ _init_ _.py는 자동으로 생성되는것!)

앱 생성 후, ==프로젝트 폴더/settings.py  INSTALLED_APPS== 에 앱을 추가해서 장고에게 이 앱을 사용하겠다고 알려야 됨(뒤에 콤마(,)찍는 것 생활화하기!!)
```
INSTALLED_APPS = [
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',
    'firstapp',
]
```

##### 데이터베이스(DB) 설정
(모델은 다음 시간에 생성)
사이트 내 데이터를 저장하기 위해서 이미 장고내에 설치되어있는 sqlite3을 사용할 것
==프로젝트 폴더/settings.py DATABASES==에서 확인가능
```
DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.sqlite3',
        'NAME': os.path.join(BASE_DIR, 'db.sqlite3'),
    }
}
```
사이트에 데이터베이스를 생성하기 위해서 터미널에 아래 코드 실행
```
>python manage.py migrate 
```
위의 코드 실행하면 ==db.sqlite3== 파일이 최상위 디렉토리안에 생성된 것을 확인할 수 있음.


##### VIEW(뷰) 코딩
VIEW는 페이지 조작할 때 수정,추가,삭제등의 기능을 설계하기위해서 필요
웹페이지에 새로운 기능을 추가하기 위해서 ==firstapp/views.py== 파일을 오픈해서 작성
```
from django.shortcuts import render
from django.views.generic.base import TemplateView

# Create your views here.
class IndexPage(TemplateView):
    template_name = "index.html"
```
* View는 클래스형/함수형 둘 중 편한 것으로 작성하면 됨
* TemplateView class : 단순한 템플릿하나 있는 클래스

##### URL 맵핑
어떤 주소에 User가 접속했을 때 어떤 뷰의 어떤 기능을 조작할 것인지 설정해 주는 작업
IndexPage모듈을 import하고 Regex(정규표현식)를 이용하여 url패턴 추가
```
from django.conf.urls import url
from django.contrib import admin
from firstapp.views import IndexPage
#url맵핑하기위해 필요한 모듈

urlpatterns = [
    url(r'^admin/', admin.site.urls),
    url(r'^$', IndexPage.as_view(), name="index"),
]
```
* 정규표현식 : ex1) r'^admin/' : url로 접속했을 때 admin/으로 시작된 모든 url을 view와 대조시켜 찾아냄.  ex2) r'^$' : 처음 메인으로 접속했을 때

위의 과정까지 마치고 터미널에서 runserver 후 localhost:8000을 실행해보면 템플릿이 존재하지 않는다는 메시지가 뜸. 

##### 템플릿 작성
User에게 보여줄 내용의 노출을 위해 화면 구성
상위 폴더 밑에 Templates 디렉토리 생성 - 그 안에 index.html 파일 생성
적절하게 타이틀이나 내용 바꿈
```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Hello Django</title>
</head>
<body>
<h1>Hello Django</h1>
</body>
</html>
```
* 파일or디렉토리 새로 추가하는경우: server를 다시 껐다 켜야함(runserver 재실행)

==settings.py TEMPLATES== 에서 'DIRS'에 아래와 같이 추가
```
'DIRS': [os.path.join(BASE_DIR,'templates')],
```
* 'DIRS'는 프로젝트 하위에 있는데 인식을 못해서..? 추가해줘야함..(설명추가필요)

위와 같은 과정을 정상적으로 마치고 나면,
![django4](https://user-images.githubusercontent.com/34964514/34639162-b43e14fe-f31d-11e7-8d15-2cfafa067f51.JPG)
웹에서 잘 뜨는지 확인하면 됨

- - -
## REVIEW
* 수업 후 바로 복습하기...3~4일 지나고 정리하려니 헷갈리는 부분 다수 존재
* 꼭꼭 설치부터 끝까지 다시한번 해보기
* 퇴근 후 피곤한데도 눈이 말똥말똥!! 재밌다! 제발 2시간내내 초집중상태로 있을  수 있길..!
* 계속 내용 수정 및 추가하기
- - -
## REFERENCE
* [장고걸스 튜토리얼(Django Girls Tutorial)](https://tutorial.djangogirls.org/ko)
* [장고 튜토리얼](https://docs.djangoproject.com/ko)
