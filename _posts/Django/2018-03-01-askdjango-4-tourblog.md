---
layout: post
title: 장고 - 기본편 4. View, Overview
categories: Django
tags: [Django, python, 장고]
comments: true
---

AskDjango_장고-기본편(여행 블로그 만들기)
## 4. View, Overview
- - -

### View 함수
> URLConf에 매핑된 Callable Object(호출가능한 객체, 함수보다 좀 더 큰 개념)
> View는 함수(Function)임
> FBV / CBV 가 있음

- 첫번째 인자로 HttpRequest 인스턴스를 받음 ex) def deleteV(request):
>HttpRequest.GET
>HttpRequest.POST
>HttpRequest.FILES
- 필히 HttpResponse 인스턴스를 리턴받아야 함
- shortcuts 모듈의 render : 템플릿에 넘겨줄 때 필요한 것 

### FBV(함수 기반 뷰) 4가지 패턴
> 처음공부할땐 FBV로 공부하다가 CBV로 넘어가기
> (FBV를 잘 사용할 수 있다면 CBV를 자유자재로 구현할 수 있을 것)

1. 직접 문자열로 HTML형식 응답하기 (HttpResponse 이용)
2. 템플릿을 통해 HTML형식 응답하기 (render 이용)
3. Json형식 응답하기 (JsonResponse 이용)
4. 파일다운로드 응답하기

### CBV(클래스 기반 뷰)
> FBV를 만들어주는 클래스가 CBV
> .as_view() 클래스함수를 통해 FBV를 생성해주는 클래스
> (ex.post_list1 = PostListView1.as_view())

##### 위의 FBV 4가지 패턴을 각각 FBV,CBV로 구현해보기
> 먼저,dojo/urls.py에 경로 지정

```
urlpatterns = [url(r'^list1/$',views.post_list1),
    url(r'^list2/$',views.post_list2),
    url(r'^list3/$',views.post_list3),
    url(r'^excel/$',views.excel_download),

    url(r'^cbv/list1/$', views_cbv.post_list1),
    url(r'^cbv/list2/$', views_cbv.post_list2),
    url(r'^cbv/list3/$', views_cbv.post_list3),
    url(r'^cbv/excel/$', views_cbv.excel_download),
]
# FBV는 dojo/view.py에, CBV는 dojo/views_cbv.py에 작성
```

> 1.직접 문자열로 HTML형식 응답하기

```
# FBV
def post_list1(request):
    name = '공유'
    return HttpResponse('''
        <h1>AskDjango</h1>
        <p>{name}</p>
        <p>여러분의 파이썬&장고 페이스메이커가 되어드리겠습니다.</p>
    '''.format(name=name))

# CBV - Type1
class PostListView1(View):
    def get(self, request):
        name = '공유'
        html = '''
           <h1>AskDjango</h1>
           <p>{name}</p>
           <p>여러분의 파이썬&장고 페이스메이커가 되어드리겠습니다.</p>
        '''.format(name=name)
        return HttpResponse(html)
post_list1 = PostListView1.as_view()

# CBV - Type2
class PostListView1(View):
    def get(self, request):
        name = '공유'
        html = self.get_template_string().format(name=name)
        return HttpResponse(html)

    def get_template_string(self):
        return '''
            <h1>AskDjango</h1>
            <p>{name}</p>
            <p>여러분의 파이썬&장고 페이스메이커가 되어드리겠습니다.</p>
        '''
post_list1 = PostListView1.as_view()
```

> 2.템플릿을 통해 HTML형식 응답하기

```
# FBV
def post_list2(request):
    name = '공유'
    return render(request, 'dojo/post_list.html',{'name':name})

# CBV
class PostListView2(TemplateView):
    template_name =  'dojo/post_list.html'

    def get_context_data(self):
        context = super().get_context_data()
        context['name'] = '공유'
        return context
post_list2 = PostListView2.as_view()
```


> 3.Json형식 응답하기

```
# FBV
def post_list3(request):
    return JsonResponse({
        'message' : '안녕',
        'items' : ['파이썬','장고'],
    }, json_dumps_params={'ensure_ascii': False})

# CBV
class PostListView3(View):  #View보다는 CRUD view를 많이 씀

    def get(self, request):
        return JsonResponse(self.get_data(), json_dumps_params={'ensure_ascii': False})

    def get_data(self):
        return {
            'message': '안녕, 파이썬&장고',
            'items': ['파이썬', '장고', 'Celery', 'Azure', 'AWS'],
        }
post_list3 = PostListView3.as_view()
```

> 4.파일다운로드 응답하기

```
# FBV
def excel_download(request):
    # filepath = '/Users/LGPC/Desktop/git_project/ask_tourblog/database_struc.xlsx'
    filepath = os.path.join(settings.BASE_DIR, 'database_struc.xlsx') #최상위 디렉토리에 저장된 엑셀파일
    filename = os.path.basename(filepath)
    with open(filepath, 'rb') as f:   #rb:read binary mode
        response = HttpResponse(f, content_type='application/vnd.ms-excel')  #기본 : text.html
        response['Content-Disposition'] = 'attachment; filename="{}"'.format(filename)
        return response

# CBV
class ExcelDownloadView(View):

    excel_path = '/Users/LGPC/Desktop/git_project/ask_tourblog/database_struc.xlsx'

    def get(self, request):
        filename = os.path.basename(self.excel_path)

        with open(self.excel_path, 'rb') as f:
            response = HttpResponse(f, content_type='application/vnd.ms-excel')
        # 필요한 응답헤더 세팅
        response['Content-Disposition'] = 'attachment; filename="{}"'.format(filename)
        return response

excel_download = ExcelDownloadView.as_view()
```

