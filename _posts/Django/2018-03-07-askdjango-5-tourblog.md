---
layout: post
title: 장고 - 기본편 5. Model, Model Fields
categories: Django
tags: [Django, python, 장고]
comments: true
---

AskDjango_장고-기본편(여행 블로그 만들기)
## 5. Model, Model Fields
- - -

### SQL(Structured Query Language)
> 장고 Model을 통해 SQL을 생성/실행할 수 있음.
> 장고 모델은 RDBMS만을 지원(관계형 데이터베이스 관리시스템)
> 장고 내장 ORM을 이용하면, SQL문을 직접 작성하지 않아도 장고 모델을 통해 데이터베이스로의 접근 가능(조회/추가/수정/삭제)
> (중요!) SQL을 작성할 줄 알아야함.(내가 짠 코드가 어떤 SQL을 만들어내는 지 검증 필요)

- - -

> <파이썬 클래스> 와 <데이터베이스 테이블>을 매핑
> Model : DB 테이블과 매핑
> Model Instance : DB 테이블의 1 row
> 하나의 데이터베이스에는 다수의 DB 테이블이 존재
> 하나의 데이터베이스 = 하나의 워크북(하나의 App안의 여러개의 table이 모인 집합이라고 생각)

### 장고 Model 커스텀 모델 정의 (특정앱/models.py)
> 먼저 DB 구조/타입을 설계한 다음에 모델 정의
> 모델 클래스명은 단수형(Post)

- - -

> Model 추가/삭제 등의 작업을 해주고, migrations - migrate 해주기
> (: 모델 내역을 DB에 테이블 생성해주는 작업)
> blog/admin.py 에서 admin에 model 등록해주기

```
# blog/models.py
from django.db import models

class Post(models.Model):
    title = models.CharField(max_length=100)  #길이 제한이 있는 문자열
    content = models.TextField()              #길이 제한이 없는 문자열(성능좋지않음)
    created_at = models.DateTimeField(auto_now_add=True)
    updated_at = models.DateTimeField(auto_now=True)

# blog/admin.py
from django.contrib import admin
from .models import Post

admin.site.register(Post)
```

### Model Fields
> 설계에 따라 필요한 타입 쓰면 됨.(document 참고)

(자주쓰는 필드 옵션)
- null(DB 옵션) : DB필드에 NULL 사용 여부(default : False)
- unique(DB 옵션) : 유일성 여부
- blank : 입력값 유효성 검사(default : False) / True인 경우, 필수적으로 입력해야함
- default : 기본적으로 입력될 값
- choices : select box 
- validators : 입력값 유효성 검사를 수행할 '함수'를 지정
  ex) 이메일만 받기, 최대길이 제한, 최소값 제한 등
- verbose_name : 필드명 설정 (한국어로 설정할 시 , 언어설정과 상관없이 한국어로 보여짐)
- help_text : 필드 입력 도움말 (ex. 포스팅 내용을 입력해주세요.)

```
# blog/models.py
import re
from django.db import models
from django.forms import  ValidationError

def lnglat_validator(value):
    if not re.match(r'^([+-]?\d+\.?\d*),([+-]?\d+\.?\d*)$', value):
        raise ValidationError('Invalid LngLat Type')

class Post(models.Model):
    title = models.CharField(max_length=100, verbose_name = '제목', help_text='포스팅 제목을 입력해주세요.')  #길이 제한이 있는 문자열
    content = models.TextField(verbose_name='내용') #길이 제한이 없는 문자열(성능좋지않음)
    tags = models.CharField(max_length=100, blank=True) #필드 필수적으로 입력할 필요 없음(blank 옵션)
    lnglat = models.CharField(max_length=50, blank=True,
        validators=[lnglat_validator],  #유효성검증 함수 넘겨줌
        help_text='위도/경도 포맷으로 입력')
    created_at = models.DateTimeField(auto_now_add=True)
    updated_at = models.DateTimeField(auto_now=True)
```


