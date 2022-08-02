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



## 내가 가진 데이터 id 전부 보는 법
```
# shell
from coplate.models import User
for u in User.objects.all():
...   print(u.id)
```


## 데이터 대량 추가
* 데이터와 관련된 json 파일을 만들고 프로젝트 바로 아래에 넣는다. (이름 : reviews.json)
* 해당하는 이미지를 media/review_pics에 넣는다.
* python manage.py loaddata reviews.json 입력한다


## 초기 페이지 다양한 기능 구현 -> ListView
```python
# views.spy
class IndexView(ListView):
    model = Review
    template_name = 'coplate/index.html'
    context_objectname = 'reviews'  # template에서 reivews라는 이름으로 부름
    paginate_by = 4
    ordering = ['-dt_created']

# urls.py
path('',views.IndexView.as_view(), name='index'),

```

## Review 자세히 보는 페이지 만들기
```python
# views.py
class ReviewDetailView(DetailView):
    model = Review
    template_name = 'coplate/review_detail.html'
    pk_url_kwarg = 'review_id'   # urls.py에서 id 받는 이름이 review_id
    
# urls.py    
path('reviews/new/', views.ReviewCreateView.as_view(), name='review-create')    
```

## Review 생성 페이지 만들기
```python
# forms.py
from .models import User, Review

class ReviewForm(forms.ModelForm):
    class Meta:
        model = Review
        # field는 models.py에서 내가 자동으로 받는 것을 제외한 전부를 적어서 넣는다.
        fields = ['title', 
                'restaurant_name',
                'restaurant_link',
                'rating',
                'image1','image2','image3',
                'content',]

        # 장고에서 라디오 형식으로 클릭하게 만들려면 아래처럼 해야됨!!
        widgets = {
            "rating":forms.RadioSelect
        }
        
# views.py
from .forms import ReviewForm

class ReviewCreateView(CreateView):
    model = Review
    form_class = ReviewForm
    template_name = 'coplate/review_form.html'
```


## 리뷰 작성자를 현재 로그인 한 사람으로 하기
```python
class ReviewCreateView(CreateView):
    model = Review
    form_class = ReviewForm
    template_name = 'coplate/review_form.html'
    
    # form valid는 데이터가 유효할때 모델 object를 만들고 저장한다.
    def form_valid(self, form):
        # author 정의하기, class면 request.user, 함수면 self.request.user
        form.instance.author = self.request.user
        return super().form_valid(form)         # ReviewCreateView에 있는 form_valid 사용해라
    
    # 성공적으로 되면 riview_id와 같이 전달해라
    def get_success_url(self):
        return reverse('review-detail', kwargs = {'review_id':self.object.id})
```

## 리뷰 수정
```python
# urls.py
path('reviews/<int:review_id>/edit', views.ReviewUpdateView.as_view(),name='review-update')

# views.py
class ReviewUpdateView(UpdaeView):
    model = Review
    form_class = ReviewForm
    template_name = 'colpate/review_form.html'
    pk_url_kwarg = 'review_id'

    # 성공적으로 되면 riview_id와 같이 전달해라
    def get_success_url(self):
        return reverse('review-detail', kwargs = {'review_id':self.object.id})

```

## 리뷰 삭제
```python
# urls.py
path('reviews/<int:review_id>/delete/', views.ReviewDeleteView.as_view(),name='review-delete'),

# views.py
class ReviewDeleteView(DeleteView):
    model = Review
    template_name = 'coplate/review_confirm_delete.html'
    pk_url_kwarg = 'review_id'

    def get_succes_rul(self):
        return reverse('index')
```
