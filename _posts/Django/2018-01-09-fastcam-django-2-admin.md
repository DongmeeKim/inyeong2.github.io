---
layout: post
title: 패캠 장고 5. Dstagram_장고 관리자 생성 및 커스터마이징
categories: Django
tags: [Django, python, 장고]
comments: true
---

패스트캠퍼스_ Python & Django를 활용한 웹 서비스 개발 CAMP 내용 정리

### Dstagram_장고 관리자 생성 및 커스터마이징
장고는 기본적으로 admin(관리자)도 제공(photo/admin.py)

#### 관리자 계정 생성
관리자 사이트에서 모든 권한을 가지는 슈퍼유저(superuser)계정 생성하는 명령
```
python manage.py createsuperuser
```
이후 Username, Email address, Password 입력(기억하기)

#### 관리자 사이트에서 photo app을 변경가능하도록 만들기
(photo/admin.py)
```
from django.contrib import admin
from .models import Photo #Photo 모델을 가져옴
admin.site.register(Photo) # 관리자에서 Photo 객체가 관리 인터페이스를 가지고 있다는 것을 명시하는 것
```
웹 브라우저에서 https://localhost:8000/admin/ 입력 후 admin사이트에 접근하여 로그인 (+photo add를 클릭해서 글도 써보기)
![admin1](https://user-images.githubusercontent.com/34964514/34916859-ee76aaae-f981-11e7-83f0-0ba8e1d2af3b.JPG)

#### 관리자 항목 커스터마이징
원래 있던 기능을 활성화하기 위한 작업(없던 기능을 추가할 수도 있음)
```
class PhotoAdmin(admin.ModelAdmin):
    list_display = ('id','author','created','updated')
    list_filter = ('author','created','updated')
    search_fields = ('text','created')
    raw_id_fields = ('author',)
    ordering = ('-updated','created')

admin.site.register(Photo, PhotoAdmin)
#(모델, 옵션모델)
```
![admincustom](https://user-images.githubusercontent.com/34964514/34916941-d3fd850c-f982-11e7-9ade-8c4b8fbab6d0.JPG)
* filter에 author가 없는 이유 : author가 한 명(admin)밖에 없기때문에 활성화되지 않음
* 정렬기준 : (1,2) & 화살표방향으로 확인가능

![admincustom2](https://user-images.githubusercontent.com/34964514/34916959-152c3c4e-f983-11e7-94a2-57e96252d057.JPG)
* author가 셀렉트박스가 아니라 검색창인 이유 : author가 많아지면 셀렉트박스로 감당하기 어렵기 때문

---
## Reference
[장고걸스 튜토리얼](https://tutorial.djangogirls.org/ko/django_models/)
[장고 튜토리얼 공식문서](https://docs.djangoproject.com/ko/2.0/intro/tutorial02/)

