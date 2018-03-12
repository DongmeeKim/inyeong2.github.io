---
layout: post
title: 장고 - 기본편 1. 장고 개발환경 구축
categories: Django
tags: [Django, python, 장고]
comments: true
---

AskDjango_장고-기본편(여행 블로그 만들기)
## 1. 개발환경 구축
- - -


### 웹 어플리케이션 기본 구조
클라이언트단 | 서버단
(웹 브라우저)  (웹 서버, 데이터베이스 서버)
(이미지)

### 장고 기본 구조
>- 지정해준 url형식에 맞지 않은 경로로 접속했을 경우 404 페이지 노출
>- 웹이기 때문에 'html'형식을 지원받음.(템플릿)
(이미지)

### 장고 개발환경 설치
원하는 디렉토리 설정 후 ( ex)./git_project/ask_tourblog )

```
# in terminal
pip install django
django-admin startproject mysite .
python manage.py migrate
python manage.py createsuperuser
python manage.py runserver
```

