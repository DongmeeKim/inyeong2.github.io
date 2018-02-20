---
layout: post
title: 패캠 장고 6. Dstagram_photo app view, url mapping, template
categories: Django
tags: [Django, python, 장고]
comments: true
---

패스트캠퍼스_ Python & Django를 활용한 웹 서비스 개발 CAMP 내용 정리

### Dstagram photo app view, url mapping, template 생성

#### (photo app) view(뷰) 생성
앞에서 Photo model을 생성함 - 여기서 만든 모델을 view에 적용하기 위해 꺼내옴.
```
from django.shortcuts import render
from .models import Photo

def post_list(request):   # request인자 : 요청한다는 것
    posts = Photo.objects.all()
    return render(request, 'photo/list.html',{'posts':posts})
```
View는 함수형과 클래스형 둘 다 생성 가능.

#### url 매핑
사용자가 어떤 url로 접속했을 때 만들어준 view(기능)을 동작시킬 것인지 설정.
이 때, `url 매핑` 이용!
- URL 매핑 : 하나의 프로젝트 내에 여러개의 App이 존재하면 메인 urls.py에서 모든걸 매핑하면 규모가 큰 프로젝트에서 힘들게 될 것. 그 대안으로, 각각의 App 안에 urls.py을 새로 생성하고 메인 urls.py에서 각 App의 urls.py파일로 URL 매핑을 위탁하게 하는 것 (2단 url매핑이라고도 함)
(자세한 건 [예제로 배우는 Python 프로그래밍](http://pythonstudy.xyz/python/article/311-URL-%EB%A7%A4%ED%95%91) 참고)

```
# photo/urls.py
from django.conf.urls import url
from . import views

urlpatterns = [
    url(r'^$',views.post_list,name='post_list'),
]

# mysite/urls.py
from django.conf.urls import url, include
from django.contrib import admin

urlpatterns = [
    url(r'^admin/', admin.site.urls),
    url(r'^$',include('photo.urls',namespace='photo')),
    url(r'^photo/', include('photo.urls')), 
]
```
`url(r'^photo/', include('photo.urls'))` : photo/로 시작되는 URL들을 photo.urls 즉 photo폴더에 있는 urls.py에 있는 매핑을 사용하겠다는 뜻.

#### template 생성
위의 작업까지 마치고 서버를 실행해 보면, ![temp_list](https://user-images.githubusercontent.com/34964514/35191821-eba84a80-fec7-11e7-92d8-5842aa60afc4.JPG)
위와 같이 템플릿이 존재하지 않는다는 에러창 뜸.
저기에서 명시해준 것 처럼 `photo/list.html` 을 만들어줘서 템플릿을 생성해줘야함.

`photo/templates/photo/` 경로에 list.html 파일 생성해주기.
(** 가상환경에서는 templates 밑에 app폴더를 새로 생성해서 템플릿파일을 만드는 것이 관례!**)
적당히 html을 바꿔주고 잘 뜨는 것을 확인하기!

## Review
url패턴 꼭 자세히 공부하기!

## Reference
[예제로 배우는 Python 프로그래밍](http://pythonstudy.xyz/Python/Django)

