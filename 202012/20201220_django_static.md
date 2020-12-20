# django static
> django에서 이미지나 js, css등 static file을 어떻게 설정하는지 확인하자.

[![NPM Version][npm-image]][npm-url]
[![Build Status][travis-image]][travis-url]
[![Downloads Stats][npm-downloads]][npm-url]

![](../header.png)

## 나는 내 프로젝트 내에 static file들을 넣어두고 개발을 진행하고, stagingi이나 production에는 project바깥에 있는 폴더를 사용해야 한다.
django project root 에 mkdir -p static_my_proj; cd static_my_proj; mkdir css js img
그리고 img폴더에 beach.jpg를 받아서 넣는다.


> project안에 img 폴더를 만들고 직접 참조하게는 할수 있다. 설정없이도 urls.py를 잘 설정하거나 또는 view에서 해당 경로를 hard-coding하면 된다.
> 하지만, 일반적인 static file은 프로젝트 외부에 두고 관리해야 한다.( cdn사용 등을 위해 nginx등 설정으로 보통 들어간다. )

- home_page.html에 {% load static %} 가 있는데도 beach.jpg가 안보인다.
- 당연히 설정한것이 없으니까 안보이는 것이다.

## 설정
django는 STATIC_ROOT, STATIC_URL 라는 환경변수를 사용한다. 따라서 STATIC_ROOT, STATIC_URL 를 설정한다.(사실 MEDIA 도 동일함. )
설정은 settings.py 에 설정하면 된다.

```python
# settings.py 화일은 project_root(src)/ecommerce/settings.py 이다.

STATIC_URL = '/static/'
# 그리고 일단 내 project내에 개별 개발자가 관리를 해야 하니 프로젝트 내 폴더 static_my_proj를 설정한다.
# 내 프로젝트에 STATIC FILE은 django에서 검색할 수 있도록 동일하게 환경변수에 설정해 준다.
# STATICFILES_DIRS 에 List로 넣어준다.
# project_root에 static_my_proj로 설정해야 하므로, os.path.dirname을 하면 ecommerce가 나오고 또 한번더 dirname을 해야 src가 나온다.

STATICFILES_DIRS = [ os.path.join(BASE_DIR, 'static_my_proj'), ]

# 이까지만 하고 home_page.html을 들어가봐도 여전히 안보인다. 왜냐면 STATIC_URL을 모르니까.
# 따라서 urls.py에 일단 기본적으로 넣어줘야 한다.
# urls.py

```python
from django.conf.urls.static import static

# 개발에서는 항상 DEBUG를 주고 하며, 실제 production에서는 다른 곳을 볼수 있다. 즉 nginx에 /static/ 등을 설정하거나 다른 cdn사이트가 될수 있으므로.
if settings.DEBUG:
    urlpatterns = urlpatterns + static(settings.STATIC_URL, document_root=settings.STATIC_ROOT)

```
이렇게 해도 여전히 안보인다.

즉, 실제 STATICFILES_DIRS 는 내가 관리하기 위한 File을 넣는 위치일 뿐이고, urls에는 STATIC_ROOT를 본다.
따라서 STATIC_ROOT를 내 project 바깥에 두고 설정한다.

나는 project 바깥에 static_cdn/static_root 라는 것으로 관리하고 싶다.
mkdir -p static_cdn/static_root

```python
# settings.py

STATIC_ROOT = os.path.join(os.path.dirname(BASE_DIR), "static_cdn", "static_root")
```

요렇게 STATIC_ROOT 를 설정하면, 위 urls.py에 설정한 STATIC_ROOT 경로도 실제로 만들어진 것이다.
근데, 이 안은 빈깡통이다.
매번 STATICFILES_DIRS 에서 STATIC_ROOT로 File을 copy해 줘야 하는데 불편하다.
django에서는 명령어를 제공한다.

```python
python manage.py collectstatic
```

이렇게 하고 일단, http://localhost:8000/static/img/beach.jpeg 로 잘 보이는지 확인하자.
잘 보인다.

그리고 home_page.html도 접속해본다. 이미지 잘 보인다.

media도 똑같이 한다.
mkdir -p static_cdn/media_root

```python
# urls.py
from django.conf.urls.static import static

# 개발에서는 항상 DEBUG를 주고 하며, 실제 production에서는 다른 곳을 볼수 있다. 즉 nginx에 /static/ 등을 설정하거나 다른 cdn사이트가 될수 있으므로.
if settings.DEBUG:
    urlpatterns = urlpatterns + static(settings.STATIC_URL, document_root=settings.STATIC_ROOT)
    urlpatterns = urlpatterns + static(settings.MEDIA_URL, document_root=settings.MEDAI_ROOT)

# settings.py
MEDIA_URL = '/media/'
MEDIA_ROOT = os.path.join(os.path.dirname(BASE_DIR), "static_cdn", "media_root")

```

<!-- Markdown link & img dfn's -->
[npm-image]: https://img.shields.io/npm/v/datadog-metrics.svg?style=flat-square
[npm-url]: https://npmjs.org/package/datadog-metrics
[npm-downloads]: https://img.shields.io/npm/dm/datadog-metrics.svg?style=flat-square
[travis-image]: https://img.shields.io/travis/dbader/node-datadog-metrics/master.svg?style=flat-square
[travis-url]: https://travis-ci.org/dbader/node-datadog-metrics
[wiki]: https://github.com/yourname/yourproject/wiki
[ssh-url]: https://arsviator.blogspot.com/2015/04/ssh-ssh-key.html
[ssh-url2]: https://junho85.pe.kr/667
