---
layout: post
permalink: /godomall-ga4-ecommerce-setting/
title: '고도몰에 구글 애널리틱스4(GA4) 전자상거래 이벤트 설정하기'
date: 2021-01-02 02:10:00 +09:00
feature: '/img/posts/009/ga4-ecommerce-setting-godo.jpg'
background: '/img/posts/009/ga4-ecommerce-setting-godo-bg.jpg'
categories:
  - marketing
tags:
  - GTM
  - GA4
  - 전자상거래
  - 고도몰
description: '고도몰에 구글 애널리틱스4(GA4) 전자상거래 이벤트를 설정하여 쇼핑몰 방문자의 쇼핑 행동을 분석할 수 있습니다.'
---

[이전 글 : 고도몰 쇼핑몰에 GA 향상된 전자상거래 추적 코드 설정하기](https://beingguru.life/godomall-enhanced-ecommerce-ga-setting/)

이전 글에서는 유니버설 애널리틱스 향상된 전자상거래 추적 코드를 고도몰5에 설정하였습니다. 이번에는 구글 애널리틱스4(GA4)에 전자상거래 이벤트를 기록할 수 있도록 설정해보겠습니다.

UA 전자상거래 설정을 완료해놓은 상태에서 GA4 설정을 추가하는 것으로 가정하고 진행하겠습니다.

1. [구글 애널리틱스4(GA4) 설치 및 User ID 설정](#1)
2. [상품 상세보기](#2)
3. [장바구니에 넣기](#3)
4. [주문서 작성](#4)
5. [결제 완료](#5)

* * *

<div id="1"></div>
## 1. 구글 애널리틱스4(GA4) 설치 및 User ID 설정

새로운 GA4 속성을 만들어 줍니다. 통화는 대한민국 원으로 설정합니다.

![GA4속성](/img/posts/009/make_ga4.jpg){: .center-image }

웹으로 데이터스트림을 새로 만들어 줍니다.

![GA4데이터](/img/posts/009/make_ga4_data.jpg){: .center-image }

![GA4데이터스트림](/img/posts/009/make_ga4_datastream.jpg){: .center-image }

스트림 세부정보를 확인할 수 있는데 측정 ID를 복사합니다.

![GA4ID](/img/posts/009/ga4_id.jpg){: .center-image }

구글 태그 매니저로 가서 이전에 만들어두었던 고도몰 컨테이너를 선택하고 아래와 같이 GA4 구성을 새로 만듭니다. 측정 ID를 입력하고 설정할 필드에 User ID도 함께 설정해 줍니다.

GA와 다르게 GA4의 User ID는 필드 이름을 user_id로 입력하고 값에는 만들어두었던 User ID 변수를 입력합니다. 이로써 구글 애널리틱스4가 설치되고 User ID 기능도 들어가게 됩니다.

![GA4TAG](/img/posts/009/ga4_tag.jpg){: .center-image }

고도몰 쇼핑몰에 방문하여 로그인하고, DebugView 메뉴에서 확인해보면 잘 적용되어 있는 것을 볼 수 있습니다.

![UserID보기](/img/posts/009/user_id_view.jpg){: .center-image }

<div id="2"></div>
## 2. 상품 상세보기

GA에 설정한 것과 방식은 비슷한데 매개변수의 이름에 다소 차이가 있습니다. 개발자 가이드 문서에서 내용을 확인할 수 있습니다.

[Ecommerce - Google Analytics 4](https://developers.google.com/analytics/devguides/collection/ga4/ecommerce)<br>
[Measure views/impressions of product/item details - Google Tag Manager](https://developers.google.com/tag-manager/ecommerce-ga4#measure_viewsimpressions_of_productitem_details)

GA에 설정할 때 productInfo 변수를 만들었던 것처럼 productInfo4라는 변수를 만들어 주고, 필요한 매개변수에 값을 넣어줍니다. 고도몰의 상품상세화면 파일로 가서 아래 코드를 이전에 작성했던 상품 상세보기 코드와 바꿔치기 해주면 됩니다.


{% highlight html %}
{% raw %}

<!-- 구글 태그 매니저 상품 상세 보기 -->
<script>
	var productInfo = [];
	productInfo.push({
	  "id": "{goodsView.goodsNo}",
	  "name": "{=goodsView['goodsNm']}",
	  "price": '{=gd_isset(goodsView['goodsPrice'],0)}'
	});

	var productInfo4 = [];
	productInfo4.push({
		'item_id': "{goodsView.goodsNo}",
		"item_name": "{=goodsView['goodsNm']}",
		"price": '{=gd_isset(goodsView['goodsPrice'],0)}'
	});

	dataLayer.push({
	  'event': 'detail',
	  'productInfo': productInfo,
	  'productInfo4': productInfo4,
	  'ecommerce': {
		'detail': {
		  'actionField': {'step': 1},
		  'products': productInfo
		 }
	   }
	});
</script>

{% endraw %}
{% endhighlight %}

구글 태그 매니저로 돌아가서 상품 정보(GA4)라는 새로운 변수를 만듭니다.

![상품정보](/img/posts/009/productinfo4.jpg){: .center-image }

이제 태그를 만들면 되는데 아래 가이드에서 GA4 이벤트 이름을 확인할 수 있습니다.

[Events: Retail/Ecommerce - Analytics Help](https://support.google.com/analytics/answer/9268036?hl=en&ref_topic=6317484)

![GA4이벤트](/img/posts/009/ga4_help.jpg){: .center-image }

상품 상세보기에 해당하는 이벤트 이름 view_item을 입력하고 아래 화면과 같이 태그를 만들어 줍니다. items 매개변수에 조금 전에 만들었던 상품 정보(GA4) 변수를 입력하고 트리거는 이전에 만들어두었던 맞춤 이벤트를 선택합니다.

![상품상세보기](/img/posts/009/view_item.jpg){: .center-image }

상품 상세페이지에 방문하고 DebugView로 확인하면 아래와 같이 정상적으로 이벤트가 찍히는 것을 볼 수 있습니다.

![상품상세보기디버그](/img/posts/009/ga4_view_item.jpg){: .center-image }

<div id="3"></div>
## 3. 장바구니에 넣기

상품 상세보기와 마찬가지로 productInfo4를 추가하여 고도몰에서 아래의 코드로 수정해 줍니다.

{% highlight html %}
{% raw %}

<!-- 구글 태그 매니저 장바구니 담기 -->
<script>
	var addToCart = function () {
		var productInfo = [];
		productInfo.push({
		  "id": "{goodsView.goodsNo}",
		  "name": "{=goodsView['goodsNm']}",
		  "price": '{=gd_isset(goodsView['goodsPrice'],0)}',
	      "quantity": goodsTotalCnt
		});

		var productInfo4 = [];
		productInfo4.push({
			'item_id': "{goodsView.goodsNo}",
			"item_name": "{=goodsView['goodsNm']}",
			"price": '{=gd_isset(goodsView['goodsPrice'],0)}',
	        "quantity": goodsTotalCnt
		});		

		dataLayer.push({
		  'event': 'addToCart',
		  'productInfo4': productInfo4,
		  'productInfo': productInfo,
		  'ecommerce': {
			'currencyCode': 'KRW',
			'add': {
			  'actionField': {'step': 2},
			  'products': productInfo
			}
		  }
		});
	}

	var cartBtnId = document.querySelector('#cartBtn');
	cartBtnId.addEventListener('click', addToCart);
</script>

{% endraw %}
{% endhighlight %}

가이드에서 장바구니 추가의 이벤트 이름을 확인합니다.

![장바구니추가](/img/posts/009/ga4_help_addtocart.jpg){: .center-image }

상품 상세보기와 마찬가지로 태그를 생성하면 장바구니 넣기까지 설정이 완료됩니다.

![장바구니추가태그](/img/posts/009/add_to_cart_tag.jpg){: .center-image }

역시 구글 태그 매니저에서 미리보기로 고도몰로 들어가 상품을 장바구니에 담으면 이벤트가 실행되고 DebugView에서도 확인되는 것을 볼 수 있습니다.

![장바구니추가미리보기](/img/posts/009/add_to_cart_preview.jpg){: .center-image }

![장바구니추가디버그](/img/posts/009/add_to_cart_debug.jpg){: .center-image }

<div id="4"></div>
## 4. 주문서 작성

여기까지 잘 따라오셨다면 여기부터는 아마 간단하게 작업이 가능할 거라고 생각됩니다. 고도몰의 주문서 작성 / 결제 파일에 작성했던 코드를 아래의 코드로 수정하면 됩니다. 마찬가지로 이전에는 없었던 productInfo4 변수를 추가하였습니다.

{% highlight html %}
{% raw %}

<script>
    var productInfo = [
      <!--{ @ cartInfo }-->
        <!--{ @ .value_ }-->
          <!--{ @ ..value_ }-->
           {
             'id': '{=...goodsNo}',
             'name': '{=...goodsNm}',
             'price': '{=...price['goodsPrice']}',
             'quantity': '{=...goodsCnt}'
           },
      		<!--{ / }-->
        <!--{ / }-->
      <!--{ / }-->    
    ];

    var productInfo4 = [
    <!--{ @ cartInfo }-->
      <!--{ @ .value_ }-->
        <!--{ @ ..value_ }-->
         {
           'item_id': '{=...goodsNo}',
           'item_name': '{=...goodsNm}',
           'price': '{=...price['goodsPrice']}',
           'quantity': '{=...goodsCnt}'
         },
        <!--{ / }-->
      <!--{ / }-->
    <!--{ / }-->        
    ];

    dataLayer.push({
      'event': 'checkout',
      'productInfo': productInfo,
      'productInfo4': productInfo4,
      'ecommerce': {
        'checkout': {
          'actionField': {'step': 3},
          'products': productInfo
       }
     },       
    });
</script>

{% endraw %}
{% endhighlight %}

이벤트 이름을 확인하고 구글 태그 매니저로 가서 아래의 태그를 만들어 줍니다.

![주문서작성태그](/img/posts/009/begin_checkout_tag.jpg){: .center-image }

<div id="5"></div>
## 5. 결제 완료

결제 완료 역시 비슷하지만 새로 만들어줄 것들이 조금 많습니다. 먼저 고도몰의 주문완료 파일에서 작성했던 코드를 아래의 코드로 바꿔줍니다.

{% highlight html %}
{% raw %}

<script>
    var transactionId = '{orderInfo.orderNo}';
    var revenue = '{=orderInfo.settlePrice}';
    var shipping = '{=gd_isset(orderInfo.totalDeliveryCharge)}';

    var productInfo = [
      <!--{ @ orderInfo.goods }-->
      {
        'id': '{=.goodsNo}',
        'name': '{=.goodsNm}',
        'price': '{=.goodsPrice}',
        'quantity': '{=.goodsCnt}'      
      },
      <!--{ / }-->
    ];

    var productInfo4 = [
      <!--{ @ orderInfo.goods }-->
      {
        'item_id': '{=.goodsNo}',
        'item_name': '{=.goodsNm}',
        'price': '{=.goodsPrice}',
        'quantity': '{=.goodsCnt}'      
      },
      <!--{ / }-->
    ];

    dataLayer.push({
      'event': 'purchase',
      'transactionId': transactionId,
      'revenue': revenue,
      'shipping': shipping,      
      'productInfo': productInfo,
      'productInfo4': productInfo4,
      'ecommerce': {
        'purchase': {
          'actionField': {
            'id': transactionId,
            'revenue': revenue,
            'shipping': shipping
          },
          'products': productInfo
        }
      }
    });    
</script>

{% endraw %}
{% endhighlight %}

가이드 문서에서 GA4 구매에 해당하는 이벤트를 확인하면 매개변수가 조금 많이 있습니다.

![구매완료이벤트](/img/posts/009/ga4_help_purchase.jpg){: .center-image }

배송비, 거래 ID, 매출액 등 dataLayer에 push 했던 데이터를 가져와서 사용하면 됩니다. 구글 태그 매니저로 가서 새로운 변수를 만들어 줍니다.

![주문번호](/img/posts/009/transactionID.jpg){: .center-image }

![매출액](/img/posts/009/revenue.jpg){: .center-image }

![배송비](/img/posts/009/shipping.jpg){: .center-image }

이렇게 만든 변수는 태그를 만들 때 입력해 주면 됩니다.

![구매완료태그](/img/posts/009/purchase_tag.jpg){: .center-image }

이렇게 모든 설정이 완료되었고, 고도몰에서 구매를 완료하면 DebugView에서도 역시 확인할 수 있는데 중요한 이벤트이기 때문에 전환 이벤트로서 다른 색깔로 나타나게 됩니다.

![구매완료미리보기](/img/posts/009/purchase_test.jpg){: .center-image }

![구매완료디버그](/img/posts/009/purchase_debug.jpg){: .center-image }

이로써 고도몰에 유니버설 애널리틱스와 더불어 구글 애널리틱스4(GA4) 전자상거래 이벤트 설정이 완료되었습니다.

![설정완료](/img/posts/009/setting.jpg){: .center-image }

이렇게 설정한 이벤트를 통해 쇼핑몰에 방문하는 사람들의 행동을 분석하여 매출 증대에 도움이 될 수 있기를 바랍니다.
