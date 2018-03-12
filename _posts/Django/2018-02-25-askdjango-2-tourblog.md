---
layout: post
title: 장고 - 기본편 2. 장고 App, URLConf, Template 
categories: Django
tags: [Django, python, 장고]
comments: true
---

AskDjango_장고-기본편(여행 블로그 만들기)
## 2. 장고 App생성, URLConf, Template 
- - -

### 장고 앱 생성 및 프로젝트에 등록
> python manage.py --help
> python manage.py startapp blog
> 이후에 settings.py 'installed_apps'에 `'blog',` 추가 : 프로젝트에 앱 등록해주기

### URLconf
```
# blog/views.py
from django.shortcuts import render

def post_list(request):
    return render(request, 'blog/post_list.html')
```

>url매핑해주기

```
# mysite/urls.py
from django.conf.urls import url, include  #include 추가
from django.contrib import admin

urlpatterns = [
    url(r'^admin/', admin.site.urls),
    url(r'blog/', include('blog.urls')),    #localhost:8000/blog/
]
```

```
# blog/urls.py
from django.conf.urls import url
from . import views

urlpatterns = [
    url(r'^$', views.post_list),  #최상위경로 localhost:8000/blog/
]
```

### Template(템플릿)
> 새로운 파일 추가/삭제했을 경우엔, 장고개발서버가 변경사항을 알아차릴 수 있도록 다시 서버 껐다켜기. (ex. html파일 추가)

```
# blog/templates/blog/post_list.html

<h1>AskDjango</h1>

<p>
첫 페이지 노출
</p>

<ul>
    <li>장고 개발환경 구축</li>
    <li>App 생성</li>
    <li>URLconf</li>
    <li>Template</li>
</ul>
```

*(28:00~):휴대폰망을 통해 접속해보기*
