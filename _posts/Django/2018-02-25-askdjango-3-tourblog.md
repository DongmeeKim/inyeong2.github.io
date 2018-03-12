---
layout: post
title: 장고 - 기본편 3. URLConf, Regular Expression(정규표현식)
categories: Django
tags: [Django, python, 장고]
comments: true
---

AskDjango_장고-기본편(여행 블로그 만들기)
## 3. URLConf, Regular Expression(정규표현식)
- - -

### 정규표현식 패턴
> 장고에서 url 라우팅시 필요한 최소한의 정규표현식 공부

- [] : 대괄호안에 있는 문자가 들어갈 수 있단 말
(실습 및 문법 추가)
- $를 붙이지 않으면 계속 그 url에 묶여버림

*>python : python shell 띄우기*
*>pip install "ipython[notebook]" : jupyter notebook 설치*
*기본 python shell 보다는 ipython 띄우기*


### URL라우팅
> settings.py에 `ROOT_URLCONF = '프로젝트.urls'` # mysite.urls (최상위 URLConf 모듈 지정)
> 장고 서버로 Http요청이 들어올 때마다, URLConf 매핑List를 처음부터 끝까지 순차적으로 훑으며 검색함.
> 매칭되는 URL Rule을 찾지 못했을 경우엔 404 Page Not Found 응답


### 새로운 APP만들고 세팅까지 마치는 과정
1. python manage.py startapp [app_name]
2. settings.py > installed_apps 에 'app_name', 추가
3. app_name/urls.py 생성 후 아래와 같은 틀 생성
    ```
    from django.conf.urls import urls

    urlpatterns = [

    ]
    ```
4. 프로젝트/urls.py(mysite/urls.py)에 include
	`url(r'app_name/', include('app_name.urls')),`


### 정규표현식 에제