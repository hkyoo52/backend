## 모델 필드
* 필드 : 데이터 테이블에서의 column
  * CharField : 문자열, 길이 지정해줘야함
  * IntergerField
  * BooleanField
  * DateField : 날짜
  * DateTimeField : 날짜 & 시간
  * EmailField
  * FileField
  * ImageField

## MOdel 작성
* 모델에 class 생성
* python manage.py makemigration : 모델을 작성했습니다.
* python manage.py migarte : 장고에 준 것을 적용해라


## migration
* migration : 데이터 구조를 변경하는것
* migrate : 데이터 구조 변경을 DB에 적용
* python manage.py showmigrations : 마이그레이션 어떻게 되어있는지 보자 (X로 되어있는 것은 실제로 적용된것)
* python manage.py sqlmigrate 앱이름(foods) 번호

## 데이터 추가하기
``` python
python manage.py shell # 쉘 들어가기
from 앱이름.models import 모델 이름   # Ex. from foods.models import Menu
Menu.objects.create(name='코딩에 빠진 닭, description ='주머니가 가벼운 당신의 마음까지 생각한 가격', price = 100000, img_path='foods/images/chicken.jpg')
Menu.objects.all()   # 무엇이 있는지 이름 볼 수 있음
Menu.objects.all().values(속성 넣어도 됨(ex.price))   # 무엇이 있는지 값 볼 수 있음
exit() #쉘 나가기
```

## CRUD
* Create : 데이터 생성
* Read : 데이터 조회
* Update : 데이터 수정
* Delete : 데이터 삭제

```python
Menu.objects.order_by('price') # 가격 순대로
Menu.objects.order_by('-price') # 가격 역순대로
Menu.objects.filter(name__contains='코')  # 이름에 '코'가 들어간 것을 찾아라
Menu.objects.filter(price__range(2000,10000))  # 가격이 2000원에서 10000원 사이인 것을 찾아라
Menu.objects.count()   # 개수 세기
Menu.objects.exclude(field=value)  # 필드에 특정 조건 제외한 데이터 조회
data = Menu.objects.get(id=1)   # id를 통해서 데이터 불러오기
data.name = '코빠닭'  # 데이터 이름 변경
data.save()  # 데이터 저장
data = Menu.objects.get(id=1)
data.delete()  # 데이터 삭제
```

## QuirySet
* DB에 있는 데이터를 파이썬이 사용하기 쉽게 변경

## 관리자 도구
```python
wsl에 python manage.py createsuperuser

# admin에다가
from 앱이름.models import 모델 이름   # Ex. from foods.models import Menu

admin.site.register(Menu)
```

* 위 과정을 하면 우리 db에 있는 내용이 admin에다가 추가됨


## db 내용 적용하기
```python
# veiws.py

from foods.models import Menu

def index(request):
  context = dict()
  menus = Menu.objects.all()
  context['menus']=menus
  return render(request, 'foods/index.html', context = context)


# index.html
{% block food-container%}
    {% for menu in menus %}
      <img src={% get_static_prefix %} {{menu.img_path}} width="300px" height="200px"/>
      <div class="info">
        <h3>{menu.name}</h3>
        <P>{{menu.description}}</p>
        <a href='#'>메뉴 보기</a>
      </div>
    {% endfor %}
  {% endblock food-container%}
