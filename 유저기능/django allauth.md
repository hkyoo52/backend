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

