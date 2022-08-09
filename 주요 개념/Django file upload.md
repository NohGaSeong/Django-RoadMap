# Django file upload
## 파일 업로드 기능 구현하기
## 1. media 경로 추가하기
[settings.py]
```.py
MEDIA_URL = '/media/'
MEDIA_ROOT = os.path.join(BASE_DIR, 'media')
```
- MEDIA_URL 과 MEDIA_ROOT 를 추가, 혹은 변경해준다.<br>
[urls.py]
```.py
urlpatterns = [
         ~~~
         ~~~
] + static(settings.MEDIA_URL, document_root=settings.MEDIA_ROOT)
````
## 2. 모델 작성하기
- 모델은 제목, 업로드 될 파일, 내용 등으로 구성될것임.
- upload_to = "", 는 MEDIA_ROOT 를 의미함.
<br>[models.py]
```.py
class FileUpload(models.Model):
  title = modles.TextFiled(max_length=40, null = Ture)
  imgFile = models.ImageField(null = True, upload_to="", blank = True)
  content = models.TextFiled()
  
  def __str__(self):
    return self.title
```

## 3. DB 반영하기
- `python3 manage.py makemigrations`
- `python3 manage.py migrate` 
- 다들 알고있는 마이그레이션 명렁어로 DB에 반영해주면된다.
## 4. 폼 작성하기
[forms.py]
```.py
class FileUploadForm(ModelForm):
    class Meta:
        model = FileUpload
        fields = ['title', 'imgfile', 'content']
```
## 5. 뷰 작성하기
```.py
def fileUpload(request):
    if request.method == 'POST':
        title = request.POST['title']
        content = request.POST['content']
        img = request.FILES["imgfile"]
        fileupload = FileUpload(
            title=title,
            content=content,
            imgfile=img,
        )
        fileupload.save()
        return redirect('fileupload')
    else:
        fileuploadForm = FileUploadForm
        context = {
            'fileuploadForm': fileuploadForm,
        }
        return render(request, 'fileupload.html', context)
```
## 6. url.py 작성하기
```.py
urlpatterns = [
	path('fileupload/', fileUpload, name="fileupload"),
]
```
## 7. 템플릿 작성하기
```.py
{% block content %}
<div class="container">
    <form method="POST" action="{% url 'fileupload' %}" enctype="multipart/form-data">
        {% csrf_token %}
        {% bootstrap_form fileuploadForm %}
        <input type="submit" class="btn btn-primary col-12" value="제출">
    </form>
</div>
{% endblock%}
```
- 아래 같은 코드를 작성해서 업로드 된 파일을 img src 로 불러와 사용자에게 보여줄 수도 있어요.
```.py
{% for list in page_obj %}
  <div class="item" style="padding-top: 15px;">
    <p style="text-align: left; font-size: 0.8em;">
      <img src="{{ list.imgfile.url }}" style="width: 180px; height: 150px;" alt=""> <br>
      <span style="font-weight: bold;">{{ list.title }}</span> <br>
      작성자: {{ list.user }} <br>
      작성일: {{ list.created_date|date:"Y-m-d" }} <br>
      조회수: {{ list.hit }}
  </p>
  </div>
{% endfor %}
```

## 글을 마치며
- 거의 블로그 하나를 복붙해놓은 수준이긴한데 좀 더 공부해보고, 실습해본다음 한번 더 수정해볼 예정이다.
