## 이론

#### 패키지
* 파이썬 파일의 모음 

#### 앱
* 장고 프로젝트를 이루는 하나의 component -> setting.py에 INSTALLED_APPS 목록에 장고 프로젝트 있음

#### Session
* 웹사이트 방문에 대한 기록
* 로그인 할 경우 Response 때 session 포함해서 보냄
* Client는 쿠키라는 곳에 session 저장, request 할때 같이 보냄
* 서버는 다시 response 할때 session 정보를 보고 로그인 유저라고 판단
* 로그아웃하면 쿠키에 session 없앰

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
  * 만약 로그인 아이디를 email로 하려면
    * ACCOUNT_AUTHENTICATION_METHOD = 'email'
    * ACCOUNT_EMAIL_REQUIRED = True  # 반드시 회원가입시 email 넣어라
  * ACCOUNT_SESSION_REMEMBER = True  # 로그인을 로그아웃할때까지 유지해라
  * SESSION_COOKIE_AGE = 36000   #  10시간동안 세션을 유지해라(그 이후에는 다시 로그인 필요) 안 사용하면 2주동안 세션 유지

## 현재 유저 정보 접근
* views.py에서는 request.user로 접근
* template에서는 {{ user }}

## Template에 user에 주는게 username이 아니라 다른것을 주고 싶으면
```python
# models.py
class User(AbstractUser):
  def __str__(self):
    return self.email    # email을 user에 해당한다.
```


## 닉네임 만들기
```python
# manage.py
class User(AbstractUser):
    nickname = models.CharField(max_length=15, unique=True, null=True)

    def __str__(self):
        return self.name
```
* admin 파일에 UserAdmin.fieldsets += (("Custom fields", {'fields' : ('nickname',)}),) 추가
* setting.py에서 ACCOUNT_SIGNUP_FORM_CLASS = "coplate.forms.SignupForm"

