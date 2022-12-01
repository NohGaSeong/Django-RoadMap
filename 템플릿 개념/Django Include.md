# Django Include

{% include %} = 템플릿 안에 템플릿 포함 가능.<br>
이는 여러 페이지에 대해 동일한 콘텐츠 블록이 있을 때 유용함.

```.html
<!-- footer.html --!>
<p>You have reached the bottom of this page, thank you for your time.</p>

<!=-- template.html -->
<h1>Hello</h1>

<p>This page contains a footer in a template.</p>

{% include 'footer.html' %} 
```
<img width="366" alt="image" src="https://user-images.githubusercontent.com/82383294/205000881-e310f643-dce8-4a7a-be03-f7d98a45cd04.png">

## 포함할 변수
`with` 키워드를 사용하여 템플릿에 변수를 보낼 수 있음. 

```.html
<!-- mymenu.html --!>
<div>HOME | {{ me }} | ABOUT | FORUM | {{ sponsor }}</div>
```

```.html
<!-- template.html -->
<!DOCTYPE html>
<html>
<body>

{% include "mymenu.html" with me="TOBIAS" sponsor="W3SCHOOLS" %}

<h1>Welcome</h1>

<p>This is my webpage</p>

</body>
</html> 
```
<img width="358" alt="image" src="https://user-images.githubusercontent.com/82383294/205001106-c46bdaac-0f9e-4389-b2d1-30faa94e3c87.png">



