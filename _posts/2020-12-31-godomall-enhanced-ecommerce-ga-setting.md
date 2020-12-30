---
layout: post
permalink: /godomall-enhanced-ecommerce-ga-setting/
title: '고도몰 쇼핑몰에 GA 향상된 전자상거래 추적 코드 설정하기'
date: 2020-12-31 02:40:00 +09:00
feature: '/img/posts/008/ga-ecommerce-setting-godo.jpg'
background: '/img/posts/008/ga-ecommerce-setting-godo-bg.jpg'
categories:
  - marketing
tags:
  - 디지털마케팅
  - 전자상거래
  - GA
  - 고도몰
description: '구글 애널리틱스의 향상된 전자상거래 기능을 활성화하여 쇼핑몰 방문자의 모든 쇼핑 활동을 추적하는 방법, step-by-step으로 알려드리겠습니다.'
---

마개이너 오갱님으로부터 많은 도움을 받았습니다.
[Ogaeng 블로그](https://ogaeng.com/)

고도몰 쇼핑몰에서 구매와 관련된 데이터를 수집할 수 있도록 구글 애널리틱스 전자상거래 추적 코드를 설정해보겠습니다. 구글 태그 매니저를 활용할 것이며 아래와 같은 순서로 진행해보겠습니다.

1. [고도몰에 구글 태그 매니저(GTM) 설치](#1)
2. [구글 애널리틱스(GA) 설치 및 User ID 설정](#2)
3. [상품 상세보기](#3)
4. [장바구니에 넣기](#4)
5. [주문서 작성](#5)
6. [결제 완료](#6)

* * *

<div id="1"></div>
## 1. 고도몰에 구글 태그 매니저(GTM) 설치하기
GTM에 로그인하여 쇼핑몰에 사용할 새로운 컨테이너를 만들어 주고 아래와 같이 구글 태그 관리자 스니펫 설치 코드가 나오는 화면을 확인해 줍니다.

![GTM스니펫](/img/posts/008/GTM_snippet.jpg){: .center-image }

상단부분의 코드를 모든 페이지에 적용되는 공통 헤더 소스 파일에 추가해야 하는데 GTM에서 기본으로 제공해 주는 코드를 고도몰에 그대로 사용하면 원치 않게 일부 코드가 치환되어 GTM이 동작하지 않는 문제가 발생합니다.

대신 아래의 코드에서 'GTM-OOOOOOO'를 자신의 GTM ID로 변경하여 상단 레이아웃에 추가해 줍니다.

{% highlight html %}
{% raw %}

<!-- Google Tag Manager -->
<script>(function(w,d,s,l,i,j){w[l]=w[l]||[];w[l].push({'gtm.start':
new Date().getTime(),event:j+'.js'});var f=d.getElementsByTagName(s)[0],
j=d.createElement(s),dl=l!='dataLayer'?'&l='+l:'';j.async=true;j.src=
'https://www.googletagmanager.com/gtm.js?id='+i+dl;f.parentNode.insertBefore(j,f);
})(window,document,'script','dataLayer','GTM-OOOOOOO','gtm');</script>
<!-- End Google Tag Manager -->

{% endraw %}
{% endhighlight %}

![헤더1](/img/posts/008/header1.jpg){: .center-image }

하단 부분의 코드는 그대로 <body>태그 바로 뒤에 붙여줍니다.

![헤더2](/img/posts/008/header2.jpg){: .center-image }

<div id="2"></div>
## 2. 구글 애널리틱스(GA) 설치 및 User ID 설정

구글 애널리틱스 관리자 페이지에서 속성 만들기로 유니버설 애널리틱스 속성을 만들어줍니다.

![UA](/img/posts/008/make_ua.jpg){: .center-image }

그리고 속성의 추적 정보 - 추적 코드 탭에서 추적 ID를 복사해두고 GTM에서 아래와 같이 유니버설 애널리틱스(UA) 추적 ID라는 새로운 변수를 하나 생성해줍니다. 향상된 전자상거래 기능 사용 및 데이터 영역 사용을 체크해 줍니다.

![UA_ID](/img/posts/008/ua_id.jpg){: .center-image }

![GTM_UA_ID](/img/posts/008/gtm_ua_id.jpg){: .center-image }

생성된 UA 추적 ID를 구글 애널리틱스 설정에 입력하고 All Pages 뷰를 트리거로 하여 구글 애널리틱스 태그를 새로 생성해 줍니다.

![UATAG](/img/posts/008/gtm_ua_create.jpg){: .center-image }

미리보기로 확인해보면 생성한 태그가 실행되고 활성화된 것을 GA에서도 확인해볼 수 있습니다.

![UAGTM완료](/img/posts/008/ga_gtm_complete.jpg){: .center-image }

확인이 되었다면 전자상거래 설정을 해줍니다. 보기 설정에서 전자상거래 설정으로 들어가 향상된 전자상거래 보고서 사용 설정까지 모두 체크해 줍니다.

전자상거래는 원래 결제만 추적하게 되는데 향상된 전자상거래를 사용하면 상품 보기, 장바구니 넣기 등 사용자의 쇼핑 행동을 여러 단계로 분석할 수 있습니다.

![전자상거래설정](/img/posts/008/ecommerce_setting.jpg){: .center-image }

다음으로 User ID를 설정해 주겠습니다. User ID를 설정하는 이유는 쇼핑몰에 로그인하는 회원이 모바일, PC 등 다른 환경에서 접속하더라도 동일한 사용자로 인식되게 하기 위함입니다.

속성의 추적 정보 - User-ID로 들어가 정책에 동의해주고 다음 단계로 넘어가 User ID 보기를 만듭니다.
User-ID 설정 시 코드를 삽입하라고 하는데 GTM을 이용할 것이기 때문에 그냥 넘어가 줍니다.

![USERID1](/img/posts/008/make_user_id.jpg){: .center-image }

![USERID2](/img/posts/008/make_user_id2.jpg){: .center-image }

이렇게 User ID를 위한 보기가 하나 만들어졌고 User ID View에서는 User ID가 할당된 하위 트래픽 관련 데이터만 표시됩니다.

그리고 맞춤 측정기준을 통해서도 User ID를 식별할 수 있도록 설정해 주겠습니다.
속성의 맞춤 측정기준에서 아래와 같이 새 맞춤 측정기준을 추가해 줍니다. 범위는 사용자로 선택합니다.

![USERID확인](/img/posts/008/user_id_check1.jpg){: .center-image }

다음으로 GTM에서 User ID를 사용할 수 있도록 dataLayer에 추가해 줄 건데 GTM 태그를 추가하였던 고도몰의 소스 파일을 수정해 주러 갑니다.

User ID로 사용될 값을 push 해야 되는데 주의사항이 있습니다. <span style="color:red;">이메일 주소나 휴대전화 번호, 이름과 같이 개인을 식별할 수 있는 값을 사용해서는 안 됩니다.</span>

고도몰에서는 치환코드를 제공해 주고 있는데 회원 일련번호를 사용하면 됩니다.

![USERID삽입](/img/posts/008/insert_user_id.jpg){: .center-image }

GTM이 실행되면서 User ID 정보를 바로 가져올 수 있도록 하기 위해 GTM 스니펫을 입력하였던 곳보다 상단에 아래 코드를 추가해 줍니다.

{% highlight html %}
{% raw %}

<script>
	if ({=gSess.memNo}) {
		dataLayer = [{
			'userId': '{=gSess.memNo}'
		}];
	}		
</script>

{% endraw %}
{% endhighlight %}

GTM으로 돌아와서 User ID 변수를 다음과 같이 만들어줍니다.

![USERID수정](/img/posts/008/gtm_user_id.jpg){: .center-image }

미리보기에서 회원가입된 아이디로 로그인을 해주면 다음과 같이 User ID 값을 받아온 것을 확인할 수 있습니다.

![GTMUSERID확인](/img/posts/008/check_gtm_user_id.jpg){: .center-image }

기존에 만들었던 UA 추적 ID 변수에서 설정할 필드 란에 이름 userId를 입력하고 값에 조금 전에 만들었던 User ID 변수를 입력해 줍니다.

맞춤 측정기준에도 User ID를 입력할 건데 GA에서 만들었던 맞춤 측정기준의 지수가 1이었기 때문에 1을 입력하고 똑같이 User ID 변수를 입력해 줍니다.

![USERID정보1](/img/posts/008/update_user_id.jpg){: .center-image }

이렇게 User ID 설정은 끝이 나고, 이제 GA의 User ID View에서 데이터를 확인할 수 있고 맞춤 측정기준에 회원 일련번호가 보이게 됩니다.

![USERIDVIEW](/img/posts/008/user_id_view.jpg){: .center-image }

<div id="3"></div>
## 3. 상품 상세보기

여기서부터는 향상된 전자상거래 기능입니다. 구글 애널리틱스와 구글 태그 매니저 개발자 가이드 문서를 제공해 주고 있으니 참고해보시기 바랍니다.

[Enhanced Ecommerce - Google Analytics](https://developers.google.com/analytics/devguides/collection/gtagjs/enhanced-ecommerce)<br>
[Measuring Views of Product Details - Google Tag Manager](https://developers.google.com/tag-manager/enhanced-ecommerce#details)

상품 상세보기에 해당하는 코드를 확인해줍니다. 여러가지 값이 있지만 상품 상세보기에 꼭 필요한 정보만 사용해보겠습니다.

![상세보기코드](/img/posts/008/view_detail_code.jpg){: .center-image }

고도몰의 상품상세화면 파일로 가서 맨 하단에 다음의 코드를 추가해줍니다. 중괄호로 묶인 부분은 각각 상품번호, 상품명, 판매가에 대한 고도몰의 치환코드입니다.

{% highlight html %}
{% raw %}

<!-- 구글 애널리틱스 상품 상세보기 코드 -->
<script>
  gtag('event', 'view_item', {
    "items": [
      {
          "id": "{goodsView.goodsNo}",
          "name": "{=goodsView['goodsNm']}",
          "price": '{=gd_isset(goodsView['goodsPrice'],0)}'
      }
    ]
  });
</script>

{% endraw %}
{% endhighlight %}

이렇게만 했다면 코드 직접 추가로 GA 세팅은 될 것입니다. 하지만 GTM을 활용할 것이기 때문에 조금 수정해 주겠습니다.

아래와 같은 형태로 dataLayer에 값을 넣어주면 되고, dataLayer에 넣어준 데이터를 GTM에서 가져와 사용하면 됩니다.

![상세보기코드GTM](/img/posts/008/view_detail_code_gtm.jpg){: .center-image }

고도몰의 상품상세화면 파일에 아래 코드를 추가해 줍니다.

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

	dataLayer.push({
	  'event': 'detail',
	  'productInfo': productInfo,
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

구매할 때까지의 퍼널을 상세보기 -> 장바구니 넣기 -> 주문서 작성 -> 결제 완료 이런 식으로 하기 위해 actionField에는 step 1을 입력해 줍니다.
추후에 GA4 및 페이스북 픽셀 등 다른 도구에서의 설정을 편리하게 하기 위해 productInfo라는 변수를 밖에서 따로 만들어주고 가져와서 사용합니다.

![상품뷰코드](/img/posts/008/goods_view_code.jpg){: .center-image }

다음으로 GTM으로 가서 새로운 트리거를 만들어줍니다.

![상세보기이벤트](/img/posts/008/event_view_detail.jpg){: .center-image }

마지막으로 태그를 만들어주면 GA 전자상거래 상품 상세보기에 대한 설정이 끝이 납니다.

![상세보기이벤트태그](/img/posts/008/ga_event_view_detail.jpg){: .center-image }

<div id="4"></div>
## 4. 장바구니에 넣기

장바구니에 넣을 때는 id, name, price 외에 수량 정보도 필요합니다. GTM 가이드 문서에 나온 것처럼 quantity를 추가하고 장바구니 버튼을 클릭했을 때 dataLayer에 담기도록 해보겠습니다.

![장바구니](/img/posts/008/add_to_cart.jpg){: .center-image }

아래 코드를 고도몰 상품상세화면 하단에 새로 추가해 줍니다.

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

		dataLayer.push({
		  'event': 'addToCart',
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

GTM에서 addToCart라고 이름 지었던 맞춤 이벤트로 트리거를 만들어줍니다.

![장바구니넣기](/img/posts/008/add_to_cart_event.jpg){: .center-image }

상품 상세보기와 마찬가지로 태그를 새로 추가해 주면 장바구니 넣기 설정이 완료됩니다.

![장바구니넣기태그](/img/posts/008/ga_add_to_cart_event.jpg){: .center-image }

<div id="5"></div>
## 5. 주문서 작성

주문서 작성 시에는 아래와 같이 장바구니에 있는 여러 제품을 한꺼번에 주문할 수도 있습니다.

![주문서작성](/img/posts/008/checkout.jpg){: .center-image }

products 값에 배열로 사용자가 주문하려는 여러 개의 상품이 모두 들어가야 합니다.

![주문서작성가이드](/img/posts/008/checkout_guide.jpg){: .center-image }

위 사진의 경우에 우리에게 필요한 productInfo 값은 다음과 같습니다.

[{id: '1000000000', name: '비잉구루', price: '1000000.00', quantity: 3},
{id: '1000000001', name: '상품 테스트', price: '5000000.00', quantity: 5}]

고도몰에서는 여러 가지 템플릿을 제공해 주고 있는데 루프를 활용하여 여러 가지 상품 정보를 한꺼번에 가져오도록 하겠습니다.

![반복](/img/posts/008/loop.jpg){: .center-image }

최종적으로는 다음의 코드를 고도몰 주문서 작성/결제 파일 하단에 추가해 줍니다.

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

    dataLayer.push({
      'event': 'checkout',
      'productInfo': productInfo,
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

![반복코드](/img/posts/008/loop_code.jpg){: .center-image }

마찬가지로 GTM에서 트리거와 태그를 만들어주면 됩니다.

![주문서작성트리거](/img/posts/008/checkout_trigger.jpg){: .center-image }

![주문서작성태그](/img/posts/008/ga_checkout.jpg){: .center-image }

GTM 미리보기로 주문서 작성 단계까지 진행을 하면 dataLayer에서 값을 확인할 수 있고 GA에서도 이벤트가 측정된 것을 확인할 수 있습니다.

![예시1](/img/posts/008/example1.jpg){: .center-image }

![예시2](/img/posts/008/example2.jpg){: .center-image }

![예시3](/img/posts/008/example3.jpg){: .center-image }

<div id="6"></div>
## 6. 결제 완료

결제 완료 단계에서는 보다 많은 정보가 필요합니다. actionField에 주문ID, 매출액, 배송비 등 다양한 매개변수가 들어있는 것을 볼 수 있습니다.

![결제완료가이드](/img/posts/008/purchase_guide.jpg){: .center-image }

우리나라의 경우 세금(tax)은 굳이 추가할 필요 없으므로 제외하고 배송비(shipping)만 상품 가격에 추가하여 매출액(revenue)으로 해줍니다.
거래ID와 매출액, 배송비 정보를 변수로 만들어주고 고도몰에서 제공하는 치환코드를 입력하여 최종적으로는 아래와 같이 주문완료 파일에 입력합니다.

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

    dataLayer.push({
      'event': 'purchase',
      'transactionId': transactionId,
      'revenue': revenue,
      'shipping': shipping,      
      'productInfo': productInfo,
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

![결제완료코드](/img/posts/008/purchase_code.jpg){: .center-image }

GTM에서 트리거와 태그를 만들어주면 끝납니다.

앞선 이벤트들은 비 상호작용 조회를 참으로 하였으나 결제 완료 이벤트는 이탈로 잡지 않아야 하기 때문에 비 상호작용 조회를 거짓으로 해줍니다.

![결제완료트리거](/img/posts/008/purchase_event.jpg){: .center-image }

![결제완료태그](/img/posts/008/ga_purchase_event.jpg){: .center-image }

이로써 상품 상세보기부터 결제 완료까지 GA의 전자상거래 기본 설정이 완료되었으며, 매출이 발생하게 되면 GA에서도 해당 데이터를 확인할 수 있습니다.

![구매완료](/img/posts/008/purchase_complete.jpg){: .center-image }

![매출액](/img/posts/008/revenue.jpg){: .center-image }

장바구니에 상품을 넣었으나 최종 결제는 하지 않은 사람들을 타겟으로 하여 광고를 보여주는 등 다양한 방법으로 마케팅에 활용할 수 있을 것입니다.

![단계](/img/posts/008/stage.jpg){: .center-image }

쇼핑몰에서 사용자가 결제를 완료하기까지의 기본적인 과정을 설정해보았는데요. 장바구니에서 삭제하거나 환불하기 등 다른 이벤트들은 상황에 따라 별도로 설정하여 사용하면 될 것 같습니다.

다음에는 GA4 설정에 대해 다뤄보겠습니다.
