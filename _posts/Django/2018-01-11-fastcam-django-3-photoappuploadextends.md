---
layout: post
title: 패캠 장고 7. Dstagram_photo app 이미지 업로드, 템플릿 확장
categories: Django
tags: [Django, python, 장고]
comments: true
---

패스트캠퍼스_ Python & Django를 활용한 웹 서비스 개발 CAMP 내용 정리

### Dstagram photo app 이미지 업로드 및 템플릿 확장

#### photo app 모델에 이미지업로드 기능 추가

제일먼저, 이미지관련 작업을 위해 필요한 pillow 라이브러리 설치 `pip install pillow`
models.py에 `photo 변수` 추가.(이미지 업로드할 수 있는 기능)

```
photo = models.ImageField(upload_to='photos/%Y/%m/%d',blank=False,default='NoImage.jpg')
```

모델에 수정/추가작업이 일어나면 makemigrations - migrate 과정을 거쳐야함.

#### media 디렉토리

위의 작업까지 마치고 어드민에서 사진을 업로드하면 photo(?photo) 폴더에 쌓이게 됨. 한두장이면 관리가 쉽겠지만 몇백,몇천장이 업로드된다면 관리가 힘들어지기 때문에 별도로 업로드되는 사진들을 관리할 수 있는 디렉토리 생성해주기

(model에선 아무것도 안바뀌는 것)

1. settings.py 제일 하단에 입력

```
MEDIA_URL = '/media/'
MEDIA_ROOT = os.path.join(BASE_DIR, 'media')
```

2. mysite/urls.py 에 urlpatterns 추가

```
urlpatterns += static(settings.MEDIA_URL, document_root=settings.MEDIA_ROOT)
```

![extends](https://user-images.githubusercontent.com/36449443/36426874-da838d12-168e-11e8-9397-7dc894fadb73.JPG)
(자꾸 에러나서 이미지로 대체)

#### Bootstrap(부트스트랩)으로 레이아웃 꾸미기

href="" "#" : 현재페이지, "/" : 메인페이지로 이동

## Review

- Noimage.jpg 다시 알아보기
- 템플릿 문법 공부