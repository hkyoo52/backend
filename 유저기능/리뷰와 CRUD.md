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
* 
