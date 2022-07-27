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
