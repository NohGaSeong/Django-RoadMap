# 쿼리셋 api 딥하게 알아보기
이번 글에선 각 쿼리셋이 어떤식으로 어떤 역할들을 하는지 서술할 예정이다.

## QuerySet의 특징
### 필터링된 쿼리셋은 고유하다
각 QuerySet은 각 필터링이 저장될 때 마다 고유한 QuerySet을 저장할 수 있다. 예를 들면 q1 에서 .all 로 모델을 불러오고 q2에서 q1을 filtering하고 q3에서 q2를 exclude 할 수 있단 소리다.
### 지연연산
장고는 lazy한 특성을 가지고 있어 코드 상에서 QuerySet을 만드는 동안에는 실제 DB에 접근을 하지 않고, 실제로 데이터가 필요한 시점에서야 접근을 한다. 그럼 언제 접근을 하여 연산을 할까?
- Slice로 Step 파라미터(:) 사용 시 (이때 [-1] 같은 음수 인덱싱은 지원하지 않는다)
  - `Post.objects.all[5:10]`
  - `Post.objects.all[:10]`
- pickling / caching
  - QuerySet을 피클 시 피클링되기전에 강제로 메모리에 로드되도록 한다.
- repr() 로 호출 시
- len() 으로 호출 시 (그러기에 단순 갯수를 파악하기 위해선 count()를 사용하자)
- list()로 변환할 때
- 해당 구문이 boolean으로 사용되었을 때(존재여부를 확인할 때는 exists()를 사용하자)

## 사용시 새로운 Queryset을 반환해주는 API
해당 메서드로 존재하는 객체가 있을 경우 QuerySet 형태로 반환한다.

### 모든 데이터 반환
데이터베이스에 있는 모든 개체의 QuerySet를 반환한다.<br>
`q1 = Post.objects.all()`

### 특정 객체 조회
filter(**kwargs) 메서드를 사용하면 원하는 조건과 일치하는 QuerySet을 반환한다.<br>
`q1 = Post.objects.filter(pk=1)`<br>
exclude(**kwargs) 메서드를 사용하면 설정한 조건과 일치하지 않는 QuerySet을 반환한다.<br>
`q1 = Post.objects.exclude(pk=1)`<br>
## 사용 시 새로운 QuerySet을 반환하지 않는 API
해당 메서드로 존재하는 객체가 있을 경우 해당 객체만 반환한다.
### 단일 객체 조회
get() 메서드를 사용하면 단일 개체만 조회가 가능하다. 만약 여러개의 객체가 존재하는 조건을 사용하면 MultipleObjectsReturned를 발생시킨다.
```python
# age 값이 20인 단일 객체 조회
queryset = Entry.objects.get(age=20)

# 바로 조회 가능한 객체 반환
>>> queryset.age
20
```
