---
layout: post
title: "패캠 장고 3. Dstagram_가상환경"
categories: Django
tags: [Django, python, 장고]
comments: true
---

패스트캠퍼스_ Python & Django를 활용한 웹 서비스 개발 CAMP 실습내용 정리

## 가상환경(Virtual Environment)
사용자가 정한 임의의 디렉토리 밑에 원하는 파일, 프로그램만 쓸수있도록 루트를 지정해놓아 독립적인 개발환경을 만들어 놓은 것. (개발의 편의성, 버전끼리 충돌방지, 프로젝트마다 가상환경을 다 따로 만들것.)

##### 가상환경 설치 ~ 프로젝트 생성과정
`dstagram2`디렉토리에서 구현된 `myenv`라는 가상환경 안에서 `mysite` 프로젝트를 생성하는 과정

![virtualenv1](https://user-images.githubusercontent.com/34964514/34914023-fd9e43a4-f94d-11e7-8ccf-0db3f25ed6c6.JPG)
![virtualenv2](https://user-images.githubusercontent.com/34964514/34914027-1bce3898-f94e-11e7-8838-79a83184e86f.JPG)

point
*  dstagram2 디렉토리안에서 dstagram2 프로젝트를 만들지 않고 mysite 프로젝트를 생성한 이유 : '디렉토리명 = 프로젝트명'일 경우 헷갈릴 수 있으므로 대부분 다르게 설정(mysite가 관례적)
*  activate시켜야 가상환경이 구동되는데, 명령어 앞 (myenv)로 구동여부를 알 수 있음
*  pycharm에서는 가상환경을 다시 깔지 않아도 됨
*  pip : 패키지 관리자 (참고:[예제로 배우는 python 프로그래밍](http://pythonstudy.xyz/python/article/504-pip-%ED%8C%A8%ED%82%A4%EC%A7%80-%EA%B4%80%EB%A6%AC%EC%9E%90))
*  pip install django==1.11 과 ~=1.11의 차이 : ==는 무조건 1.11 버전만 설치해달라 / ~=1.11는 1.11과 가장 가까운 최신버전을 설치해달라
*  앞선 실습(프로젝트1)과 차이점 명확히 알기

---
## REFERENCE
- [예제로 배우는 Python 프로그래밍](http://pythonstudy.xyz/python/article/303-Django-%EC%84%A4%EC%B9%98)


