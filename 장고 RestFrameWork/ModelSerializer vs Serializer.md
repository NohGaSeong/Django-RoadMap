## Serializer 과 ModelSerializer의 차이
- Serializer과 ModelSerializer는 Django의 Form과 ModelForm와 유사.

```.py
# forms.py(ModelForm 사용시)
from django.forms import ModelForm
from .models import Post
class PostForm(ModelForm):
    class Meta:
        model = Post
        fields = '__all__'
```
```.py
# serializers.py(ModelSerializer 사용 시)
from rest_framework.serializers import ModelSerializer
from .models import Post
class PostSerializer(ModelSerializer):
    class Meta:
        model = Post
        fields = '__all__'
```
- 서로의 공통점? 필드 지정 및 모델로부터 읽어올 수 있고, 입력된 데이터에 대한 유효성 검사를 진행
- 차이점으로는 Form과 ModelForm는 form 태그가 포함된 HTML을 생성하지만, Serializer과 ModelSerializer는 form
데이터가 포함된 JSON 타입의 문자열을 생성한다.

