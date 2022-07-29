## 이론
#### django
* field에 몇개의 값중 1개만 들어갈때 사용
* field에 들어갈 choice를 리스트 형태로 만들고 그 리스트를 choices 옵션에 넣어준다.
* Form을 사용하면 choice 중 하나 고르게 한다.
* 이후 불를때는 (모델에 오브젝트).필드.초이스  <- 이런식으로 부름

#### ForeignKey
* 모델과 모델간의 관계 모델링
* 1 : N 관계를 모델링함 (N의 역활)
* on_delete 파라미터에 따라 유저가 삭제시 전부 삭제할것인지, 유저 이름을 null값으로 바꿀건지 결정


## 리뷰 기능 구현
#### 이메일 관련 유효성 검사를 위해서
```python
# validators.py 
def validate_restaurant_link(value):
  if 'place.naver.com' not in value and 'place.map.kakao.com' not in value:
      raise ValidationError('place.naver.com 또는 place.map.kakao.com이 들어가야 합니다.')
```

```python
# models.py

from .validators import validate_restaurant_link

class Review(models.Model):
  title = models.CharField(max_length=30)
  restaurant_name = models.CharField(max_length=20)
  restaurant_link = models.URLField(validators=[validate_restaurant_link])

  RATING_CHOICES = [
      (1,1),
      (2,2),
      (3,3),
      (4,4),
      (5,5),
  ]
  rating = models.IntegerField(choices=RATING_CHOICES)

  image1 = models.ImageField()
  image2 = models.ImageField()
  image3 = models.ImageField()
  content = models.TextField()
  dt_created = models.DateTimeField(auto_now_add=True)
  dt_updated = models.DateTimeField(auto_add=True)

  def __str__(self):
      return self.title
```

## Media 넣는 법
```
# setting.py
MEDIA_ROOT = os.path.join(BASE_DIR, 'media')
MEDIA_URL = '/uploads/'
```
```python
# urls.py
from django.conf import settings
from django.conf.urls.static import static

urlpatterns += static(settings.MEDIA_URL, document_root=settings.MEDIA_URL)
```

## ImageFeild 다루기
* pip install pillow
```python
# models.py
class Review(models.Model):
    ~~~
    image1 = models.ImageField(upload_to='review_pics')
    image2 = models.ImageField(upload_to='review_pics', blank=True)
    image3 = models.ImageField(upload_to='review_pics', blank=True)
    ~~~
```
```python
# admin.py

admin.site.register(Review)
```
* admin 사이트에서 리뷰 사용 가능!!
* 이후 리뷰에 들어간 데이터를 다룰려면 r = Review.objects.all().first() 라고 두고 r로 다루면 됨
    * img = r.image1
    * url = r.image1.url


## 모델과 모델 사이에 관계 만들기
* models.py에 Review 안에 author = models.ForeignKey(User, on_delete=models.CASCADE) 추가 -> 이후 author로 불러올 수 있음
* makemigrations, (변경 사항이 있으면 1을 넣고 나서 id 정해줘) , migrate
```
# shell
from coplate.models import Review, User

Review.objects.filter(author__id=1)  # id=1 인거 찾아
Review.objects.filter(author__nickname=='환규')  # 환규가 작성한거 찾아
```





