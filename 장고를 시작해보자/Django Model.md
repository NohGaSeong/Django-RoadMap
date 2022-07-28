# Django Model
## 우선 모델이란?
> 어플리케이션이 '무엇'을 할지 정의 어플리케이션의 정보 DB, data 처리 로직 컴포넌트
- 어떤 동작을 수행하는 코드
- 사용자 쿼리에 대한 데이터 제공
- 데이터 업데이트
- 상태가 업데이트되면 등록된 '모든' 뷰에 자신의 상태 변화를 알림
- 뷰나 컨트롤러를 직접 참조해서 업데이트를 반영하면 안되고 업데이트 통지 처리 방법만 구현
- 모델은 여러 개의 뷰를 가질 수 있음
## Django의 Model이란?
- 이 모델을 저장하면 그 내용이 데이터베이스에 저장되는 것이 Django 모델의 특별한 점.
- 예제 코드와 함께 살펴보자
### 예제코드
<a href = "https://tutorial.djangogirls.org/ko/django_models/">코드 출처</a>
```.py
from django.conf import settings
from django.db import models
from django.utils import timezone


class Post(models.Model):
    author = models.ForeignKey(settings.AUTH_USER_MODEL, on_delete=models.CASCADE)
    title = models.CharField(max_length=200)
    text = models.TextField()
    created_date = models.DateTimeField(
            default=timezone.now)
    published_date = models.DateTimeField(
            blank=True, null=True)

    def publish(self):
        self.published_date = timezone.now()
        self.save()

    def __str__(self):
        return self.title
```
- class Post(models.Model):는 모델을 정의하는 코드입니다. (모델 = 객체)
- class는 이제부터 객체를 정의하겠다 ~ 라는 것을 알려줌.
- Post는 모델의 이름입니다.
- models은 Post가 장고 모델임을 의미합니다. 이 코드 때문에 장고는 Post가 데이터베이스에 저장되어야 한다고 알게 됩니다.

#### 속성 정의
- 속성을 정의하기 위해, 필드마다 어떤 종류의 데이터 타입을 가지는지를 정해야 합니다.
- 여기서 데이터 타입에는 텍스트, 숫자, 날짜, 사용자 같은 다른 객체 참조 등이 있습니다.
- 모델의 필드와 정의하는 방법은 <a href = "https://docs.djangoproject.com/en/2.0/ref/models/fields/#field-types">여기</a>를 봐주세요.

#### 클래스 안에 있는 def?
- 클래스 안에 있는 def = 메서드(method) 입니다. (클래스 내에 있는 함수라고 생각하시면 편해요)
- 아래있는__str__ 메서드를 봅시다. 코드를 봐보면 `__str__`를 호출하면 Post 모델의 제목 텍스트(string)를 얻게 될 거에요.

### 모델을 위한 테이블 만들기
- 변화가 생겼다는 것을 장고에게 알려주기 = `python manage.py makemigrations <app name>`
- 생긴 변화를 데이터베이스에 반영하기 = `python manage.py migrate <app name>`
