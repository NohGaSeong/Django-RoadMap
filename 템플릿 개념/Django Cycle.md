# Django Cycle
{% cycle %} 태그를 사용하면 다양한 반복에 대해 다양한 작업을 수행할 수 있음.
태그는 인수를 사용하고 첫 cycle번째 반복은 첫 번째 인수를 사용하고 두 번째 반복은 두 번째 인수를 사용함.

### 예시
```.html
<ul>
  {% for x in members %} #members = views에서 member 모델을 가져온 뷰.
    <li style = 'background-color: {% cycle 'lightblue' 'pink' 'yellow' 'coral'
      {{ x.firstname }}
    </li>
  {% endfor %}
</ul>
```
<img width="341" alt="image" src="https://user-images.githubusercontent.com/82383294/204996832-956b499d-74bc-4050-88cc-9812bfe9665b.png">

## 인수를 변수로 순환
첫 번째 예에서 인수 값은 주기에 직접 표시되었지만 인수 값을 변수에 보관하고 나중에 사용할 수도 있음.
```.html
<ul>
  {% for x in members %}
    {% cycle 'lightblue' 'pink' 'yellow' 'coral' 'grey' as bgcolor silent %}
    <li style='background-color:{{ bgcolor }}'>
      {{ x.firstname }}
    </li>
  {% endfor %}
</ul> 
```
<img width="341" alt="image" src="https://user-images.githubusercontent.com/82383294/204997366-a90726f7-bdf5-45cc-b89b-c5d857c0e9cd.png">

silient 키워드 = 추가하지 않으면 인수 값이 출력에 두 번 표시됨.
아래는 silient 키워드가 없을때의 결괏값
<img width="336" alt="image" src="https://user-images.githubusercontent.com/82383294/204997405-8eabff4c-bf70-49ad-a8fc-0b87c4c87e41.png">

## 재설정 주기
{% resetcycle %} = 주기를 강제로 다시 시작할 수 있음.

```.html
<!-- 3회 사이클 이후 사이클을 다시 시작함. -->
<ul>
  {% for x in members %}
    {% cycle 'lightblue' 'pink' 'yellow' 'coral' 'grey' as bgcolor silent %}
    {% if forloop.counter == 3 %}
      {% resetcycle %}
    {% endif %}
    <li style='background-color:{{ bgcolor }}'>
      {{ x.firstname }}
    </li>
  {% endfor %}
</ul> 
```
<img width="332" alt="image" src="https://user-images.githubusercontent.com/82383294/204997713-461fab11-74d3-4898-983f-09163fd2b4d7.png">

