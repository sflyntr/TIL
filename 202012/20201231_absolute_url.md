# get_absolute_url
> django에서 url을 관리하는 get_absolute_url을 분석하자.

[![NPM Version][npm-image]][npm-url]
[![Build Status][travis-image]][travis-url]
[![Downloads Stats][npm-downloads]][npm-url]

![](../header.png)

## 일반적으로 model에 get_absolute_url을 정의해서 ui에서 model의 instance 데이터에 접근할때 많이 사용함.
> model에 get_absolute_url 함수를 만든다.
> get_absolute_url를 만들때 보통 reverse 를 사용한다.

단순히 return "/products/{slug}/".format(slug=self.slug) 이런식으로 할수도 있다.
return reverse("products:detail", kwargs={"slug": self.slug}) 이렇게 많이 한다.

```
products.urls.py에는 
url(r'^(?P<slug>[\w-]+)/$', ProductDetailSlugView.as_view(), name='detail')

메인의 urls.py에는
url(r'^products/', include("products.urls", namespace='products'))
```

즉, reverse는 urls.py로 역으로 해당 url을 찾아내는 것이다. 직접 url경로를 "/.../..."로 설정하는 것이 아니다.
따라서, 위의 경우 products:detail 은 namespace:name 의 형식으로 products:detail 이 되고,
이를 통해 products/<slug>/ 가 url이 나오는 것이다. 그리고 kwargs를 통해 slug=xxx 로 함수를 호출하면,
최종 url은 products/xxxx/ 로 return이 된다.

이는 중복제거와 함께 url관리를 urls.py로 통합해서 사용할 수 있으므로 대부분 이렇게 사용한다.

<!-- Markdown link & img dfn's -->
[npm-image]: https://img.shields.io/npm/v/datadog-metrics.svg?style=flat-square
[npm-url]: https://npmjs.org/package/datadog-metrics
[npm-downloads]: https://img.shields.io/npm/dm/datadog-metrics.svg?style=flat-square
[travis-image]: https://img.shields.io/travis/dbader/node-datadog-metrics/master.svg?style=flat-square
[travis-url]: https://travis-ci.org/dbader/node-datadog-metrics
[wiki]: https://github.com/yourname/yourproject/wiki
[ssh-url]: https://arsviator.blogspot.com/2015/04/ssh-ssh-key.html
[ssh-url2]: https://junho85.pe.kr/667
