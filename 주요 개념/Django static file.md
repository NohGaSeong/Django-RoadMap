# 장고 정적 파일 처리
## Static file?
개발 리소스로서의 정적인 파일들이다. (js, css, image)

## Static file Path
프로젝트 전반적으로 사용되는 static 파일들은 `project_root/app/static/app` 경로에 저장되며 장고 앱에서 사용하는 static 파일들은 `settings.STATICTIFLES_DIRS` 라는 지정된 경로에 있다.

## Static setting
static file과 관련된 세팅들을 살펴보자.

**STATIC_URL** : 정적 파일 요청이 오가는 URL

**STATIC_DIRS** : 장고의 File System Loader에 의해 참조되는 디렉토리 설정이다. 이 경로에서 정적 파일을 찾는다.

**STATIC_ROOT** : python manage.py collectstatic(여러 direcotry로 흩어진 정적 파일들을 설정된 경로에 복사해준다.) 명령이 참조하는 설정이다. 

### 왜 python manage.py collectstatic을 해줘야하냐?
외부 웹서버가 Finder의 도움 없이도 static의 파일을 서빙하기 위한 작업임. (장고에선 static 파일을 직접 처리하는 기능을 담고있지 않기 때문)

### 정적 디렉토리 목록 구성
장고의 App Directories Finder에 의해 앱들의 정적 파일 경로들(project_root/app/static)이 정적 파일 디렉토리 목록에 추가된다. 장고의 File System Finder에 의해 settings.STATICFILES_DIRS 설정한 디렉토리 목록을 정적 파일 디렉토리 목록에 추가된다.

## 템플릿과 활용해보기
- {% load static %} : static 모듈을 임포트 한다.
- {% static 'jquery-3.6.0.min222.js' %} : jquery 정적 파일 주소를 동적으로 가져온다.
```html
{% load static %}

<!DOCTYPE html>
<html lang="ko">
<head>
  <link rel="stylesheet" href="{% static 'bootstrap-4.6.1-dist/css/bootstrap.css' %}">
  <script src="{% static 'jquery-3.6.0.min222.js' %}"></script>
</head>
```
