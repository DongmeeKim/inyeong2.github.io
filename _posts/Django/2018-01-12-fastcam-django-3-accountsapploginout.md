---
layout: post
title: 패캠 장고 8. Dstagram_accounts app 로그인, 로그아웃 기능
categories: Django
tags: [Django, python, 장고]
comments: true
---

패스트캠퍼스_ Python & Django를 활용한 웹 서비스 개발 CAMP 내용 정리

### Dstagram accounts app Login / Logout 기능 
Accounts App : 로그인, 로그아웃 등 회원정보 저장을 위해 필요한 App 

#### accounts app 생성 후 login/logout 기능 생성
터미널에 `python manage.py startapp accounts` 실행 후 `mysite/settings.py` installed_apps 에 accounts 추가.
이후 accounts app 안에 `urls.py` 를 생성하여 아래의 코드를 침

```
from django.conf.urls  import url,include
urlpatterns = [url(r'^',include('django.contrib.auth.urls')), ]
```

`mysite/urls.py`의 urlpatterns에 `    url(r'accounts/',include('accounts.urls')),` 추가 후 `localhost:8000/accounts/login`을 주소창에 치면, 아래와 같은 메시지가 뜬다.
![login_temp](https://user-images.githubusercontent.com/34964514/35481958-3dd3e0ae-0471-11e8-9f33-c20f543a5500.JPG)

위의 메시지에 따라 `registration/login.html` 파일을 생성해야함. 
(accounts/templates/registration에 login.html , logout.html 파일 생성하면 됨)

login , logout html파일에 적당히 내용 입력(base.html 템플릿확장을 통해)
csrf_token은 보안사항과 관련된 것이므로 꼭 추가해주기

마지막으로 `accounts/urls.py` 수정
```
from django.conf.urls  import url,include
from django.contrib.auth import views as auth_views

urlpatterns = [
    url(r'^login/$', auth_views.LoginView.as_view(), name='login'),
    url(r'^logout/$', auth_views.LogoutView.as_view(template_name='registration/logout.html'), name='logout'),
]
```
(name='' : 템플릿에서 지칭할 명칭 정해주는 것)

#### login decorator(데코레이터) 기능 추가
**decorator?**
함수형 뷰에 접근제한을 설정해놓고 싶을때 사용하는 기능(클래스형 뷰에는 mixin)

`photo/views.py`에서 데코레이터기능을 추가하고싶은 post_list 메서드에 로그인제한기능 설정(로그인을 하지 않은 상태면 게시물이 보이지 않고 계속 로그인페이지가 뜨도록!)
```
from django.contrib.auth.decorators import login_required

@login_required
def post_list(request):
    posts = Photo.objects.all()
    return render(request, 'photo/list.html',{'posts':posts})
```
(로그인에 성공하면 메인페이지가 뜨도록 하는 설정 추가하려면, `settings.py`에 LOGIN_REDIRECT_URL = '/' 추가)

#### base.html 템플릿 수정
기존에 부트스트랩에서 긁어오면서 Home, About, Contact과 같은 기본적인 탭들을 내가 필요한 탭으로 수정하고자 함.

`/templates/base.html`에서 아래와 같이 수정
![basecode](https://user-images.githubusercontent.com/34964514/35482199-fcd166e0-0474-11e8-82b6-6ef89c307a5e.JPG)

(로그인 전)
![mainlogout](https://user-images.githubusercontent.com/34964514/35482233-5a986120-0475-11e8-9a5d-0098e3b73892.JPG)

(로그인 후)
![base](https://user-images.githubusercontent.com/34964514/35482168-a1907848-0474-11e8-9768-b6271e5ea1a2.JPG)
