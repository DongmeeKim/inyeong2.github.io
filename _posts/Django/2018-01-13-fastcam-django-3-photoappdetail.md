---
layout: post
title: 패캠 장고 9. Dstagram_photo app detail view(디테일 뷰)
categories: Django
tags: [Django, python, 장고]
comments: true
---

패스트캠퍼스_ Python & Django를 활용한 웹 서비스 개발 CAMP 내용 정리

### Dstagram photo app Detail view 

#### 상세 이미지 페이지 생성 (Detail view)
하나의 개체(이미지)만을 따로 볼 수 있는 페이지를 생성하려고 함. 

**생성과정**
1. mysite/urls.py 에 url 추가 `url(r'^photo/',include('photo.urls')),`
2. photo/urls.py에 url패턴 추가 
3. 템플릿 생성(`templates/photo/detail.html`)
4. runserver 후 `./photo/single/[post_id]`로 접속해서 상세페이지가 뜨는지 확인

#### 공부
- generic view(제너릭 뷰)
- CRUD