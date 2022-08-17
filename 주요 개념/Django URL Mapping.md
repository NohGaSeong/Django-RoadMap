# Django URL Mapping
### Django가 요청을 처리하는 방법
1. 사용할 루트 URLConf 모듈을 결정.
2. 해당 Python 모듈을 로드하고 변수를 찾음.
3. 각 URL 패턴을 순서대로 실행하고, 요청된 URL과 일치하는 첫번째 패턴에서 중지
4. 일치하면 Python 함수인 지정된 views 를 가져오고, 호출
5. URL 패턴이 일치하지 않거나, 이 과정에서 예외가 발생하면 적절한 오류 처리 보기를 호출해줌.

### 예시
- 아래는 샘플 URLConf 임.
```.py
 from django.urls import path
 from . import views
 
 urlpatterns = [
  path('articles/2003/', views.special_case_2003),
  path('articles/<int:year>/', views.year_archive),
  path('articles/<int:year>/<int:month>/', views.month_archive),
  path('articles/<int:yaer>/<int:month>/<slug:slug>/', views.article_datail),
]

```
- URL에서 값을 캡처(?) 하려면 <> 를 사용함.
- 캡처된 값은 선택적으로 변환기 유형을 포함 가능. <int:name> = 정수 매개변수를 캡처하는데 사용. 포함되지 않은 경우 문자를 제외한 모든 문자열이 / 와 일치
예시 요청:
- `/articles/2005/03/` 하면 세 번째 항목과 일치 = views.month_archive 를 호출. views.month_archive(request, year=2005, month=3)
- '/articles/2003' 은 두번째도 되지만 첫번째에서 통과하기 때문에 views.special_case(request) 를 호출함.

### 경로 변환기
- str : '/'를 제외하고 비어있지 않은 모든 문자열과 일치. 변환기가 식에 포함되지 않은 경우 이 값이 기본값.
- int : 0또는 양의 정수와 일치하면 int 를 반환.
- slug : ASCII 문자 또는 숫자, 하이픈 및 밑줄 문자로 구성된 모든 슬러그 문자열과 일치하여야함. 예시로 `building-your-1st-django-site.`
- uuid : 형식이 지정된 UUID 와 일치하여야함. 여러 URL 이 동일한 페이지에 매핑되는 것을 방지하려면 - 를 포함하여야하고 문자는 소문자여야함. 예시로 `0424234234-234e134-349fgf314-243c9cc`
- path : '/' 포함하여 비어있지 않은 모든 문자열과 일치. 이렇게 하면 str 과 같이 URL 경로의 세그먼트가 아닌 전체 URL 경로와 일치시킬 수 있음.

### 다른 URLconf 포함
```.py
from django.urls import include, path

urlpatterns = [
  path('community/', include('aggregator.urls')),
  path('contact/', include('contact.urls')),
]
```
- Django가 include() 를 만날때마다 URL의 해당 지점고 일치하는 부분을 잘라내고 나머지 문자열을 추가 처리를 위해 포함된 URLconf로 보냄.

### include() 어찌씀?
- include(module, namespace=None)
- include(pattern_list)
- include(pattern_list,app_namespace),namespace=None)
- 대략적인 사용법은 위와 같고 최상위 urlconf에서 urlpattern들을 적다보면 앱이 하나씩 추가될수록 코드가 지저분해지고 관리하기 수월하지 않을 수 있기에 사용한다. 
- Application 디렉토리 안에 urlconf를 할 수 있게 urls.py 파일을 만들어주고 프로젝트 최상위 urlconf(urls.py)에서는 해당 모듈을 바라보게 설정해준다.

## URL 관련 규칙
- 역시나 RESTful API 이름 짓는 규칙을 참고하여서 일관성있는 이름을 지을 것을 공식에서 권장하고 있다.

### 문서
`http://api.example.com/device-management/managed-devices/{device-id}`
### 집합
`http://api.example.com/device-management/managed-devices`
### 창고
`http://api.example.com/cart-management/users/{id}/carts`
### 컨트롤러
`http://api.example.com/cart-management/users{id}/cart/checkout`
### 예시들 
- 목록 : `/blog/posts/
- 생성 : `/blog/new/`
- 보기 : `/blog/posts/1/`
- 수정 : `/blog/posts/1/edit/`
- 삭제 : `/blog/posts/1/delete/`
- 객체와 무관한 메소드 : `/blog/posts/view-name/`
- 객체와 관련 있는 메소드 : `/blog/posts/1/view-name/`

### 유의사항
- 계층구조를 표현하기 위해 / 를 사용
- url끝에는 / 를 붙이지 않는다.
- 가독성을 위해 _ 대진 - 를 사용한다.
- 모든 문자는 소문자로 통일한다.
