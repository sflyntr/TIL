# django static
> django에서 이미지나 js, css등 static file을 어떻게 설정하는지 확인하자.

[![NPM Version][npm-image]][npm-url]
[![Build Status][travis-image]][travis-url]
[![Downloads Stats][npm-downloads]][npm-url]

![](../header.png)

## 나는 내 프로젝트 내에 static file들을 넣어두고 개발을 진행하고, stagingi이나 production에는 project바깥에 있는 폴더를 사용해야 한다.
django project root 에 mkdir -p static_my_proj; cd static_my_proj; mkdir css js img
그리고 img폴더에 beach.jpg를 받아서 넣는다.

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

STATICFILES_DIRS = [ os.path.join(BASE_DIR, 'static_my_proj'), ]

# 이까지만 하고 home_page.html을 들어가봐도 여전히 안보인다.



# 따라서 project_root에 static_my_proj로 설정해야 하므로, os.path.dirname을 하면 ecommerce가 나오고 또 한번더 dirname을 해야 src가 나온다.
# 그게 바로 BASE_DIR 로 기본적으로 설정되어 있다.
# BASE_DIR = os.path.dirname(os.path.dirname(os.path.abspath(__file__)))

#

STATIC_ROOT = os.path.join(BASE_DIR, 'static_my_proj') # 이렇게 하면 "/"까지 붙어서 연결된다.

```

## file mode

ssh화일들은 매우 중요한 화일이라 화일의 권한설정도 매우 중요하다.   
아래처럼 해라.   

즉 모든 화일은 644라 하되, 개인키(id_rsa)만 권한을 더 빼서 600으로 할것. 디렉토리(.ssh)는 700 요렇게 기억하면 된다.  

```bash
chmod 700 ~/.ssh
chmod 600 ~/.ssh/id_rsa
chmod 644 ~/.ssh/id_rsa.pub  
chmod 644 ~/.ssh/authorized_keys
chmod 644 ~/.ssh/known_host
chmod 400 ~/.ssh/config # 요 config는 권한이 더 낮다.
```

공개키 서버로 옮길때는 아래처럼 하는것이 일반적이다.  

```bash
scp $HOME/.ssh/id_rsa.pub yogiyo@deliveryhero.co.kr:id_rsa.pub
cat $HOME/id_rsa.pub >> $HOME/.ssh/authorized_keys
# ssh-copy-id 도 많이 사용한다.
```

아래 링크에 설명이 잘되어 있다.  
[SSH설명][ssh-url]
[비밀번호없이 SSH접속][ssh-url2]






<!-- Markdown link & img dfn's -->
[npm-image]: https://img.shields.io/npm/v/datadog-metrics.svg?style=flat-square
[npm-url]: https://npmjs.org/package/datadog-metrics
[npm-downloads]: https://img.shields.io/npm/dm/datadog-metrics.svg?style=flat-square
[travis-image]: https://img.shields.io/travis/dbader/node-datadog-metrics/master.svg?style=flat-square
[travis-url]: https://travis-ci.org/dbader/node-datadog-metrics
[wiki]: https://github.com/yourname/yourproject/wiki
[ssh-url]: https://arsviator.blogspot.com/2015/04/ssh-ssh-key.html
[ssh-url2]: https://junho85.pe.kr/667
