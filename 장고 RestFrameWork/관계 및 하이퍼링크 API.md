# 관계 및 하이퍼링크 API 
## API 루트에 대한 엔드 포인트 작성
- snippet , user 에 대한 엔드포인트가 있지만 api에 대한 단일 진입점이 없음.
- 하나를 만들기 위해 함수 기반 뷰와 @api_view 데코레이터를 사용할것임.

```.py
from rest_framework.decorators import api_view
from rest_framework.response import Response
from rest_framework.reverse import reverse


@api_view(['GET'])
def api_root(request, format=None):
    return Response({
        'users': reverse('user-list', request=request, format=format),
        'snippets': reverse('snippet-list', request=request, format=format)
    })
# 정규화된 URL을 반환하기 위해 reverse 기능을 사용하고있음.
```
## 강조 표시된 Snippet에 대한 엔드포인트 작성
- API 에서 여전히 누락된건 코드의 엔드포인트.
- 다른 모든 API 엔드 포인트와 달리 JSON을 사용하지 않고 HTML 표현만 제시함..
- DRF에서 제공하는 두 가지 스타일의 HTML 렌더러가 있음.
- 하나의 템플릿을 사용, 렌더링이 된 HTML 처리, 다른 하나는 사전 렌더링 된 HTML을 처리.
- 구체적인 일반 뷰를 사용하는 대신 인스턴스를 나타내는 데 기본 클래스를 사용하고 자체 .get() 메서드를 만듬.

```.py
from rest_framework import renderers

class SnippetHighlight(generics.GenericAPIView):
    queryset = Snippet.objects.all()
    renderer_classes = [renderers.StaticHTMLRenderer]

    def get(self, request, *args, **kwargs):
        snippet = self.get_object()
        return Response(snippet.highlighted)
```
## API 하이퍼링크
- 엔터티 간의 관계를 다루는 것은 웹 API 디자인에서 가장 어려운 측면 중 하나.
- 관계를 나타내기 위해 선택할 수 있는 여러가지 방법이 있음.
- 기본키 사용, 엔터티 간 하이퍼 링크 사용, 관련 엔터티에서 고유 식별 슬러그 필드를 사용, 관련 엔터티의 기본 문자열 표현을 사용, 부모 표현 내에 관련 엔터티 중첩, 다른 사용자 지정 표현 등등.
- DRF는 위와 같은 모든 스타일을 지원. 정방향, 역방향 관게에서 적용하거나 일반 외래 키와 같은 사용자 정의 관리자에 적용 할 수 있음.
- 이 경우에 엔터티간에 하이퍼 링크 스타일을 사용하려고함. 이를 위해 기존 `ModelSerializer` 대신 `HyperlinkedModelSerializer`를 확장하도록 serializer를 수정함.
  ### modelserializer이랑 hyperlinkedmodelserializer 랑은 차이점은 뭐가 있을까
  - 기본적으로 id 필드는 포함되지 않음.
  - HyperlinkedIdentifyField를 사용하는 url 필드를 포함함.
  - 관계는 `PrimaryKeyRelatedField` 대신 `HyperlinkedRelatedField`를 사용.

- 하이퍼 링크를 사용하기 위해 기존 serializer 를 쉽게 다시 작성할 수 있음.
```.py
class SnippetSerializer(serializers.HyperlinkedModelSerializer):
    owner = serializers.ReadOnlyField(source='owner.username')
    highlight = serializers.HyperlinkedIdentityField(view_name='snippet-highlight', format='html')

    class Meta:
        model = Snippet
        fields = ['url', 'id', 'highlight', 'owner',
                  'title', 'code', 'linenos', 'language', 'style']


class UserSerializer(serializers.HyperlinkedModelSerializer):
    snippets = serializers.HyperlinkedRelatedField(many=True, view_name='snippet-detail', read_only=True)

    class Meta:
        model = User
        fields = ['url', 'id', 'username', 'snippets']
```
## URL 패턴의 이름이 지정되어 있는지 확인
- 하이퍼링크된 API를 사용하려면 URL 패턴의 이름을 지정해야함. 이름을 지정해야하는 URL 패턴을 알아보자
  - API의 루트는 `user-list` 및 `snippet-list`를 나타낸다.
  - snippet serializer 에는 `snippet-highlight`를 나타내는 필드가 포함
  - user serializer는 `snippet-detail`을 나타내는 필드를 포함
  - snippet , user serializer 에는 기본적으로 `{model_name}-detail`을 참조하는 url 필드가 포함되있는 경우 `snippet-detail` 및 `user-detail`
- 그래서 모든 이름은 URLConf에 추가한 최종 urls.py 파일은 아래와 같음.
```.py
from django.urls import path
from rest_framework.urlpatterns import format_suffix_patterns
from snippets import views

# API endpoints
urlpatterns = format_suffix_patterns([
    path('', views.api_root),
    path('snippets/',
        views.SnippetList.as_view(),
        name='snippet-list'),
    path('snippets/<int:pk>/',
        views.SnippetDetail.as_view(),
        name='snippet-detail'),
    path('snippets/<int:pk>/highlight/',
        views.SnippetHighlight.as_view(),
        name='snippet-highlight'),
    path('users/',
        views.UserList.as_view(),
        name='user-list'),
    path('users/<int:pk>/',
        views.UserDetail.as_view(),
        name='user-detail')
])
```

## Adding pagination
- user 및 snippet에 대한 목록보기는 많은 인스턴스를 return 할 수 있으므로 실제로 결과를 페이지 매김하고 API 클라이언트가 
각 개별 페이지를 단계별로 실행하도록하고 싶습니다.
- `settings` 파일을 약간 수정하여 페이지 매김을 사용하도록 기본 목록 스타일을 변경할 수 있음. 아래와 같은 설정 추가
```.py
REST_FRAMEWORK = {
    'DEFAULT_PAGINATION_CLASS': 'rest_framework.pagination.PageNumberPagination',
    'PAGE_SIZE': 10
}
```


