# Serializer
## Serializer 의 뜻?
- Django Rest Framework 를 사용하면 Serializer 라는 개념이 나옴.
- Serializer의 뜻 ? : 직렬화 _ 공간적으로 동시에 존재하는 상태로 표현되어 있는 데이터를 이것에 대응하는 직렬인 상태로 존재하도록 변환한다. 라는 의미.
- 쉽게 말하자면 모델 인스턴스를 REST API 에서 사용하는 JSON 의 형태로 바꿔주는것임.

## 에제로 알아보자
```.py
# models.py
class Journalist(models.Model):
    first_name = models.CharField(max_length=60)
    last_name = models.CharField(max_length=60)
    
class Article(models.Model):
    author = models.ForeignKey(Journalist, on_delete=models.CASCADE)
    title = models.CharField(max_length=120)
    description = models.CharField(max_length=120)
    body = models.TextField(max_length=500)
    location = models.CharField(max_length=50)
    publication_date = models.DateField()

    active = models.BooleanField(default=False)

    created_at = models.DateTimeField(auto_now_add=True)
    updated_at = models.DateTimeField(auto_now=True)
```
- 우선 models.py에서 Journalist, Article 모델을 생성.
- 이때 Journalist는 기자의 정보를 저장하는 모델이고, Article은 기사를 저장하는 모델

```.py
# serializers.py
from rest_framework import serializers
from news.models import Article, Journalist


class ArticleSerializer(serializers.ModelSerializer):

    class Meta:
        model = Article
        exclude = ('id',)


class JournalistSerializer(serializers.ModelSerializer):

    article_set = serializers.HyperlinkedRelatedField(many=True, read_only=True, view_name='article-detail')

    class Meta:
        model = Journalist
        fields = '__all__'
```
- ArticleSerializer는 Article 모델 인스턴스에서 id를 제외한 모든 field의 값을 직렬화하라는 역할을 가진 클래스입니다. 
- JournalistSerialzer는 Journalist 모델 인스턴스에서 모든 field의 값을 직렬화하라는 역할을 가진 클래스입니다.
- article_set은 Journalist를 참조하고 있는 모델의 값을 Hyperlink의 형태로 볼 수 있도록 해주는 값입니다. 
- article_set은 (클래스 이름)_ set으로 설정되어 있는 값 입니다. 
- 따라서 다른 값을 사용하고 싶다면 models.py에서 realted_name을 따로 설정해주는 것으로 해결할 수 있습니다. 더 자세한 사항은 restframework 의 공식홈페이지를 참고해주세요.

 
