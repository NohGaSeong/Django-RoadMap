# Django Form
## Form 이란?
- HTML에서 적어도 한 개 이상의 type="submit"인 input 요소를 포함하는 <form>...</form> 태그 사이의 요소들의 집합으로 정의된다.
- Form 의 속성은 action 과 method 로 분류된다.

### Action
- Form이 submit될 때 처리가 필요한 데이터를 전달받는 곳의 자원/URL 주소. 미설정시 현재 페이지 URL로 다시 제출된다.
### Method
- HTTP methond (POST / GET)
### <a href = "https://velog.io/@zihs0822/Form-%EC%9D%B4%EB%9E%80">예제 코드</a>
```.html
<form action="/team_name_url/" method="post">
    <label for="team_name">Enter name: </label>
    <input id="team_name" type="text" name="name_field" value="Default name for team.">
    <input type="submit" value="OK">
</form>
```
- type : 어떤 종류의 위젯이 표시될지 정의
- name/id : JavaScript/CSS/HTML에 있는 필드를 확인하는데 사용
- value : 필드가 표시되는 초기값 정의
- label : 사용자 인터페이스(UI) 요소의 라벨을 정의. for 속성에 다른 요소의 id를 입력하여 결합
- submit : 모든 input 요소의 데이터가 서버로 업로드

## Django Form
### <a href = "https://tutorial.djangogirls.org/ko/django_forms/">예제 코드</a>
```.py
from django import forms

from .models import Post

class PostForm(forms.ModelForm):

    class Meta:
        model = Post
        fields = ('title', 'text',)
```
- class Meta : 폼을 만들기 위해서 어떤 model이 쓰여야 하는지 장고에 알려주는 구문입니다.
- 필드에선 title 과 text 만 보여지게.
- 이제 뷰 에서 이 폼을 사용해 템플릿에서 보여주기만 하면 됨.


### Django Form이 주요하게 다루는 사항
- 사용자가 처음으로 폼을 요청할 때 기본 폼을 보여준다.
- 제출 요청으로 부터 데이타를 수집하고 그것을 폼에 결합한다.
- 데이타를 다듬어서 유효성을 검증한다.
- 입력된 어떤 데이타가 유효하지 않다면, 폼을 다시 표시한다.
- 입력된 모든 데이타가 유효하다면, 요청된 동작을 수행한다.
