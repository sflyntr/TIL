1. base.html 에 script를 작성한다.
2. carts/snippets/remove-product.html 을 새로 만든다.
3. /cart/api/를 통해 json data를 전달할 cart_detail_api_view 를 작성한다.
4.  urls.py 를 작성한다.
5. update-cart.html 에 form class 설정. 과 data-endpoint도 설정하고 span class 설정한다.
6. carts. views.py 에 is_ajax call인 경우 json return하도록 한다.
7. Product.views 에 ProductListView에 cart_obj를 context에 넣는걸로 수정한다.
   ( 기존에는 product list 에 cart정보를 안보여줬다. )
8. 보이지 않는 form(cart-item-remove-form)을 생성하고 안보이게 한다.(cart home.html)
9. Cart home.html를 손댄다.
10. Navbar 에 카트수 ajax 갱신되도록 한다.
11. Product list 에 카드담기/삭제 넣는다.(카드리스트로 보이니 card.html)
