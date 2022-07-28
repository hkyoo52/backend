#### 패키지
* 파이썬 파일의 모음 

#### 앱
* 장고 프로젝트를 이루는 하나의 component -> setting.py에 INSTALLED_APPS 목록에 장고 프로젝트 있음

## django-allauth
* 일반적으로 django-allauth에 urls, views, forms 사용하고 django.contrib.auth에서 models 사용한다.
#### django-allauth
* 유저 기능을 구현하는데 쓰임
  * urls
  * views
  * forms

#### djnago.contrib.auth
* 유저 기능을 구현하는데 쓰이는 장고에 포함된 앱
  * user model -> models
  * login/logout -> urls
  * login logic -> views, forms

## django.contrib.auth
* User : 기본 유저 모델 -> 권장 X, migrate 하기 어려움
* AbstractUser : 상속 받아서 쓸 수 있는 유저 모델 -> 장고에서 기본적인 틀을 제공
* AbstractBaseUser : 상속 받아서 쓸 수 있는 유저 모델 -> 처음부터 구현할 User를 쓸거면 이거를 대신 사용해라

#### 사용법
```python
# models.py
from django.db import models
from django.contrib.auth.models import AbstractUser

# Create your models here.
class User(AbstractUser):
    pass
```
* setting.py 맨 아래에 AUTH_USER_MODEL = 'coplate.User' 추가
* makemigrations, migrate

## admin 생성 및 설정
```python
# admin.py
from django.contrib import admin
from django.contrib.auth.admin import UserAdmin
from .models import User

# Register your models here.
admin.site.register(User, UserAdmin)
```
* 이후에 python manage.py createsuperuser

#### 설정
* pip install django-allauth
* django-allauth install 홈페이지에서 authentication_backends을 setting.py 맨 아래에 복붙
* INSTALLED_APPS 에서 필수부분 복붙
* 혹시 social login 할거면 installed_app에서 해당하는 부분 복붙
* SITE_ID 복붙
* setting.py 맨 아래에 EMAIL_BACKEND = 'django.core.mail.backends.console.EmailBackend' 붙인다.
* urls.py에 urlpatterns도 가져오라 -> 이때 경로는 원하는데로 바꿔도 됨(account/ 수정 가능)

## login 만들기
* 앱의 urls.py에 path("", include('프로젝트이름.urls')), 넣기
* 프로젝트에 urls.py 생성
```python
from django.urls import path
from . import views

urlpatterns = [
    path('',views.index, name='index'),
]
```
* 프로젝트에 templates/coplate/index.html 생성
* setting.py에 아래 2줄 복붙
  * ACCOUNT_SIGNUP_REDIRECT_URL = 'index'
  * LOGIN_REDIRECT_URL = 'index'
  * ACCOUNT_LOGOUT_ON_GET = True
  * 
## 현재 유저 정보 접근
* views.py에서는 request.user로 접근
* template에서는 {{ user }}
* 
