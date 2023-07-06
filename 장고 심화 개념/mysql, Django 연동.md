# mysql, Django 연동
DB 생성 등의 과정은 다 건너뛰고 연동 부분만 살펴보겠다.
## 기존 Django settings의 database
![image](https://github.com/NohGaSeong/Django-RoadMap/assets/82383294/67351a25-94b8-4784-ae82-191583b27a83)
## MySQL을 연결했을때 Django의 settings
```python
DATABASES = {
    'default' : {
        'ENGINE': 'django.db.backends.mysql',
        'NAME': 'DB 명',
        'USER': 'root',
        'PASSWORD': 'DB 접속용 비밀번호',
        'HOST': '127.0.0.1',
        'PORT': '3306',
    }
}
```

연동을 해준 후 기존 DB와 마찬가지로 `python3 manage.py makemigrations`, `python3 manage.py migrate` 를 진행해주어야한다.

## 새로만든 DB가 아닌 기존 DB를 불러와 적용시키고 싶을때
| MySQL을 연결했을때 Django의 settings
의 부분을 진행시켜준 뒤 터미널에 `python3 manage.py inspectdb > '저장 하고자하는 위치' /models.py` 를 입력하게되면
```
class Users(models.Model):
    user_id = models.CharField(db_column='USER_ID', primary_key=True, max_length=50)  # Field name made lowercase.
    user_nm = models.CharField(db_column='USER_NM', max_length=200)  # Field name made lowercase.
    tel_no = models.CharField(db_column='TEL_NO', max_length=50, blank=True, null=True)  # Field name made lowercase.
    email = models.CharField(db_column='EMAIL', max_length=100, blank=True, null=True)  # Field name made lowercase.
    compny_nm = models.CharField(db_column='COMPNY_NM', max_length=200, blank=True, null=True)  # Field name made lowercase.
    dept_nm = models.CharField(db_column='DEPT_NM', max_length=200, blank=True, null=True)  # Field name made lowercase.
    jdeg_nm = models.CharField(db_column='JDEG_NM', max_length=200, blank=True, null=True)  # Field name made lowercase.
    working_site_nm = models.CharField(db_column='WORKING_SITE_NM', max_length=200, blank=True, null=True)  # Field name made lowercase.
    reg_tm = models.DateTimeField(db_column='REG_TM')  # Field name made lowercase.
    chg_tm = models.DateTimeField(db_column='CHG_TM')  # Field name made lowercase.

    class Meta:
        managed = False
        db_table = 'users'
```
이런식으로 ORM을 알아서 짜준다.
