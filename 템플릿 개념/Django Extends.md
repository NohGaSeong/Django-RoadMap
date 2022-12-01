# Django Extends
{% extends %} 태그를 사용 = 템플릿에 대한 상위 템플릿을 추가할 수 있음.<br/>
즉, 다른 모든 페이지의 부모 역할을 하는 하나의 마스터 페이지를 가질 수 있음.

### 예시
```.html
<!-- mymaster.html --!>
<html>
<body>

<h1>Welcome</h1>

{% block mymessage %}
{% endblock %}

</body>
</html> 
```

```.html
<!-- template.html --!>
{% extends 'mymaster.html' %}

{% block mymessage %}
  <p>This page has a master page</p>
{% endblock %} 
```
<img width="233" alt="image" src="https://user-images.githubusercontent.com/82383294/205000495-f1e1bd77-345c-4c6d-af3e-9c2da0d989ad.png">

{% block %} = placeholder를 만들어줌.



