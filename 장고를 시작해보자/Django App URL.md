# Django App URL
## 장고 URL은 어떻게 작동할까?
- 우선 아래의 예제는 프로젝트를 만들면있는 기본값의 urls.Py 파일이다.
```.py
from django.contrib import admin
from django.urls import path

urlpatterns = [
    path('admin/', admin.site.urls),
]
```
- 위의 예제대로라면 장고는 admin/로 시작하는 모든 URL을 view와 대조해 찾아냄
- 무수히 많은 URL이 admin URL에 포함될 수 있어 일일이 모두 쓸 수 없기에 `정규표현식` 을 사용함.

## URL을 만들어보자.
- mysite/urls.py파일을 깨끗한 상태로 유지하기 위해, polls 애플리케이션에서 메인 mysite/urls.py파일로 url들을 가져올거임.
```.py
from django.contrib import admin
from django.urls import path, include

urlpatterns = [
    path('admin/', admin.site.urls),
    path('', include('polls.urls')),
]
```
### polls.urls
- 아래와 같은 코드를 작성함
```.py
from django.urls import path
from . import views

urlpatterns = [
    path('', views.polls_list, name='polls_list'),
]
```
- 딱히 mysite/urls.py 랑 다를게 없음.
#### 코드 설명
- 이 URL 패턴은 빈 문자열에 매칭이 되며, 장고 URL 확인자(resolver)는 전체 URL 경로에서 접두어(prefix)에 포함되는 도메인 이름을 무시하고 받아들입니다.
- 이 패턴은 장고에게 누군가 웹사이트에 'http://127.0.0.1:8000/' 주소로 들어왔을 때 views.polls_list를 보여주라고 말해줍니다.
- 마지막 부분인 name='polls_list'는 URL에 이름을 붙인 것으로 뷰를 식별합니다. 뷰의 이름과 같을 수도 완전히 다를 수도 있습니다.
- 앱의 각 URL마다 이름 짓는 것은 중요합니다. URL에 고유한 이름을 붙여, 외우고 부르기 쉽게 만들어야 해요. (왜 중요한지는 추후 추가 정리 예정)

