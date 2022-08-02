# Django Template Language
- 아래의 코드는 장고 공식문서에 나와있는 예제이다. 이 예제와 함께 각 기능들을 알아보자.
```.html
{% extends "base_generic.html" %}

{% block title %}{{ section.title }}{% endblock %}

{% block content %}
<h1>{{ section.title }}</h1>

{% for story in story_list %}
<h2>
  <a href="{{ story.get_absolute_url }}">
    {{ story.headline|upper }}
  </a>
</h2>
<p>{{ story.tease|truncatewords:"100" }}</p>
{% endfor %}
{% endblock %}
```
## Variable
- 뷰에서 템플릿으로 객체를 전달 가능함.
- {{ Variable }} 같은 모양으로 생겼으며 (.) 은 변수의 속성에 접근할때 사용한다.
- 위의 예제에서{{ section.title}}부분을 봐보자. 뷰에서 section객체를 html 문서로 보내 title 속성을 출력할 수 있도록 하는 것이다.
## Filter
- 변수의 값을 특정 형식으로 변환할 때 사용한다. 변수 다음에 (|) 를 넣고, 적용하고자 하는 필터를 적어준다.

### 필터의 특징
- 동시에 여러 개의 필터를 사용 가능하다.
  - {{ text|escape|linebreaks }}는 텍스트 컨텐츠를 이스케이프한 다음, 행 바꿈을 `<p>` 태그로 바꾸기 위해 종종 사용되곤 한다.
- : 문자를 통해 인자를 취하는 몇몇 필터가 있다.
  - {{bio|truncatewords:30 }} 처럼 사용하는데 이것은 bio 변수의 처음 30 단어를 보여준다.

- 더 많은 템플릿 필터를 <a href = "https://docs.djangoproject.com/en/4.0/ref/templates/builtins/#ref-templates-builtins-filters">여기</a>에 나와있다.
## Tag
- 모양은 {% tag %} 이렇다.
- 일부 태그는 {% extends %} 이렇게 단일로 쓸 수도 있지만 {% if %} ... {% endif %} 같이 끝마친다는것을 알려주는 종료 태그가 필요한 태그도 있다.
- 몇 가지 태그를 알아보도록 하자

### for
- 배열의 각 항목을 반복합니다. 예를 들어 다음에서 제공되는 선수목록을 표시하려면 다음을 수행합니다 athlete_list.
```.html
<ul>
{% for athlete in athlete_list %}
    <li>{{ athlete.name }}</li>
{% endfor %}
</ul>
```
### if/else
- 위의 경우 비어 있지 않으면 변수 athlete_list로 선수의 수가 표시된다.
- 비어 있지 않으면 "Athletes must be out..."라는 메시지가 표시되고, 두 목록이 모두 비어 있으면 "선수 없음" 이라고 표시된다.
- 필터와 다양한 연산자도 사용 가능하다.
```.html
{% if athlete_list %}
    Number of athletes: {{ athlete_list|length }}
{% elif athlete_in_locker_room_list %}
    Athletes should be out of the locker room soon!
{% else %}
    No athletes.
{% endif %}
```
## Comment
- 주석이다. 
- 모양은 {#..#} 이렇다.
- 여러줄은 {% comment %} ... {% endcomment %} 로 작성하면된다.
