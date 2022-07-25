## 정적 파일
* 웹페이지를 rendering하는 과정에서 필요한 추가적인 파일 Ex. CSS, JS ,PNG< FONT

#### 정적 파일 구조
* 앱 이름
  * static
    * 앱 이름


## Template 언어
* 템플릿 변수 : {{변수명.속성}}
  * 우리가 지정한 데이터로 변환
  * view에서 넘겨 받은 값으로 변환
* 템플릿 필터 : {{변수명|필터}}
  * 템플릿 변수를 특정 형식으로 변환
* 템플릿 태그 : {% 태그 %} {% end태그 %}
  * logic에 사용 Ex. {% for %} {% endfor%} 
  * 상속 처리 Ex. {% block %} {% endblock %}
* 주석 : {# 주석 #}


## 상속
* 기본적인 토대는 부모클래스로 만든다. (안바뀌는 부분)
* 바뀌는 부분은 자식 클래스로 만든다.
* 사용법 : {% block %} {% endblock %}

```python
# base.html 파일
{% load static %}
<!DOCTYPE html>
<html>
<head>
  <title>코스토랑</title>
  <meta charset="utf-8">
  <link rel="stylesheet" href={% static 'foods/css/styles.css' %}>
</head>
<body>
  <div>  
    {% block date-block%}
    {% endblock date-block%}
  </div>
  <hr/>
 ~~~~~~
 
# index.html 파일
{% extends  './base.html' %}
{% load static%}

{% block date-block %}
  <div>12 Aug, 2020<div>
{% endblock date-block %}
```

## 동적 웹페이지 만들때
* views에서 데이터를 옮길때는 dict 형태로!!
* render에 3번째 파라미터에 context = dict 이름   <= 이런 형식으로 전송

## 웹페이지 여러개 만들어서 연결하기
* urls.py에 urlpatterns에 새로운 path를 추가해라 Ex. path(' ', ---)
* index에 href 부분에 바뀌는 곳에 주소를 넣어라 Ex. <a href='/foods/chicken'>메뉴 보기</a>

#### 다이나믹 URL
```python
# urls.py
urlpatterns = [
    path('index/', views.index),
    path('menu/<str:food>', views.food_detail)  # meny/파라미터 로 전송, food_detail은 파라미터를 받음
   ]

# views.py
def food_detail(request,food):
    context = {'name':food}
    if food == 'chicken':
        context['name'] = '코딩에 빠진 닭'
        context['description'] = '주머니가 가벼운 당신의 마음까지 생각하는 가격 !'
        context['price']=10000
    return render(request, 'foods/detail.html',context=context)


# detail.html
{% load static %}
<h2>{{name}}</h2>
<div>{{description}}</div>
<p>{{price}} 원 </p>
```


* 위에처럼 path를 만든다.


## 에러페이지
* 200 : 정상
* 204 : 정상이지만 서버에 보내줄 데이터 없음
* 301 : 요청한 자원이 새로운 주소로 옮겨짐
* 304 : 요청에 대한 변경 사항이 없음
* 403 : 요청한 자원에 대한 접근 권한 없음
* 404 : 요청한 자원이 없음
* 500 : 서버 내부 에러
