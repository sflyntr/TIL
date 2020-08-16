# Django QuerySet

# Django shell

> ModelClass.objects => 이것은 ModelManager 이다.  
> 쿼리셋이 아니다.  
> 즉, ModelClass.objects.all() 이렇게 하면 쿼리셋이 나온다.
> 모델매니저는 기본적으로 models.Model을 상속하는데 그 안에 all() 등 함수가 정의되어 있다.
> 이걸 models.ModelManager를 상속받은 클래스에서 재정의 할수 있다.  
> 요약하면 objects 은 쿼리셋이 아니고 모델매니저이다. 착각하지 말자.  


```
from payo.chaipay.models import Chaipay  
qs_cpay= Chaipay.objects.filter(payment_no=20200816145815005678)  
type(Chaipay.objects) # <class 'django.db.models.manager.Manager'>  
type(qs_cpay) # <class 'django.db.models.query.QuerySet'>  
type(qs_cpay.first()) # <class 'payo.chaipay.models.Chaipay'>  

from payo.payment.models import Payment  
Payment.objects.all()[:10] # limit 10 과 같다.   
qs_pay =  Payment.objects.filter(payment_no=20200816145815005678) # QuerySet 
qs_pay.first() # 결과(Payment의 객체배열)  
[f.get_attname() for f in Payment._meta.fields] # column 리스트.  
qs_pay.values_list() # select * from payment where payment_no = 20200816145815005678 과 동일한데 컬럼과 매핑이 없다.
# 가장 많이 사용할 만한 것.
qs_pay.values() # 컬럼명:값 이렇게 하여 모든 컬럼이 다 나온다.  
```


쿼리들을 같이 보고 싶을떄는   

```
./manage.py shell
qs_pay =  Payment.objects.filter(payment_no=20200816145815005678) # QuerySet 
qs_pay.first() # 결과(Payment의 객체배열)  
print(qs_pay[:10].query)

# 또는 아래처럼 하면 된다고 하는데 나는 안된다.  
./manage.py shell_plus --print-sql


```
