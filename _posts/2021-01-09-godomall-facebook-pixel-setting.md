---
layout: post
permalink: /godomall-facebook-pixel-setting/
title: '고도몰 전자상거래 이벤트 페이스북 픽셀 설치하기(with GTM)'
date: 2021-01-09 03:20:00 +09:00
feature: '/img/posts/010/godomall-facebook-pixel-setting.jpg'
background: '/img/posts/010/godomall-facebook-pixel-setting-bg.jpg'
categories:
  - marketing
tags:
  - 고도몰
  - 페이스북픽셀
  - 전자상거래
  - 디지털마케팅
description: '구글 태그 매니저를 이용해서 고도몰에 페이스북 픽셀을 설치하여 쇼핑몰 방문자의 쇼핑 이벤트를 추적하는 방법'
---

[이전 글 : 고도몰에 구글 애널리틱스4(GA4) 전자상거래 이벤트 설정하기](https://beingguru.life/godomall-ga4-ecommerce-setting/)

고도몰5 쇼핑몰 방문자의 행동을 분석하기 위해 GA4와 유니버설 애널리틱스 향상된 전자상거래 추적 코드를 설치하였는데요. 이번에는 구글 태그 매니저를 활용하여 페이스북 픽셀을 설치하고 상품 상세보기부터 구매 완료까지의 쇼핑 이벤트를 추적할 수 있도록 해보겠습니다.

페이스북 픽셀을 생성하고 고도몰에 구글 태그 매니저가 아직 설치되어 있지 않다면 아래 글을 참고하여 설치하도록 합니다.

[이전 글 : 고도몰 쇼핑몰에 GA 향상된 전자상거래 추적 코드 설정하기](https://beingguru.life/godomall-enhanced-ecommerce-ga-setting/)

목차는 아래와 같습니다.

1. [페이스북 픽셀 설치](#1)
2. [상품 상세보기](#2)
3. [장바구니에 넣기](#3)
4. [주문서 작성](#4)
5. [결제 완료](#5)

* * *

<div id="1"></div>
## 1. 페이스북 픽셀 설치

이벤트 관리자에 데이터 소스 탭으로 가서 픽셀 코드를 복사합니다.

![픽셀](/img/posts/010/pixel.jpg){: .center-image }

![픽셀2](/img/posts/010/pixel2.jpg){: .center-image }

구글 태그 매니저에서 새로운 태그를 생성하고 All Pages 뷰를 트리거로 하여 맞춤 HTML에 복사한 코드를 붙여 넣으면 페이스북 픽셀 기본 설치는 끝입니다.

![태그매니저픽셀](/img/posts/010/tagmanager-pixel.jpg){: .center-image }

크롬 브라우저에서 Facebook Pixel Helper 확장 프로그램을 설치하면 페이스북 픽셀이 제대로 작동하고 있는지 확인하기 편리합니다.

![픽셀헬퍼](/img/posts/010/googleextpixel.jpg){: .center-image }

<div id="2"></div>
## 2. 상품 상세보기

상품 상세보기 이벤트 코드를 추가할 건데 페이스북 개발자 페이지에서 어떤 이벤트를 사용하는 것이 적절할지 확인해 줍니다.

[Facebook 픽셀 참고 자료](https://developers.facebook.com/docs/facebook-pixel/reference)

위 참고 자료에서 표준 이벤트를 살펴보면 ViewContent라는 이벤트가 있습니다. 상품이나 어떤 콘텐츠를 볼 때는 이 표준 이벤트를 사용하면 됩니다.

![viewcontent](/img/posts/010/viewcontent.jpg){: .center-image }

상품 상세보기를 할 때에는 어떤 상품을 보는지가 중요할 텐데 여기에서는 content_ids, content_type 등의 개체 속성값이 중요합니다. 그 중에 가장 중요한 속성은 고유한 값을 가지는 content_ids 입니다. 또한 상품인지 혹은 상품 목록인지를 정해주기 위한 content_type도 중요합니다. 다이내믹 광고를 하려면 content_type과 contents 혹은 content_ids가 필수적입니다.

다이내믹 광고는 제품을 아직 구매하지 않았거나 특정 제품에 관심을 보인 타겟에게 행동을 완료하도록 유도할 수 있어서 전자상거래 비즈니스에 효과적입니다. 필요하다면 꼭 설정해 주도록 합니다.

개체 속성에 대한 설명은 아래에서 확인할 수 있습니다.

![contentid](/img/posts/010/contentid.jpg){: .center-image }

content_ids 값은 배열을 입력할 것입니다.

이전 글에서 구글 태그 매니저에 상품 정보(GA4)라는 변수를 만든 적이 있습니다. 해당 변수를 활용하여 상품 ID 배열 변수를 새로 만들어 주겠습니다.

아래 코드를 맞춤 자바스크립트에 입력합니다. 상품 정보(GA4)에서 item_id 라는 키값을 가져와 배열로 사용하겠다는 코드입니다.  상품 정보(GA4) 변수명을 다르게 만들었다면 해당 변수명을 입력하면 됩니다.

{% highlight javascript %}
{% raw %}

function() {
  var key = {{상품 정보(GA4)}};
  var list = [];
  for(var i = 0; i < key.length; i++) {
      var name = key[i].item_id;
      list.push(name);
  }
  return list;
}

{% endraw %}
{% endhighlight %}

![ids](/img/posts/010/ids.jpg){: .center-image }

마찬가지로 상품 이름도 변수로 새로 만들어 주겠습니다.

{% highlight javascript %}
{% raw %}

function() {
  var key = {{상품 정보(GA4)}};
  var list = [];
  for(var i = 0; i < key.length; i++) {
      var name = key[i].item_name;
      list.push(name);
  }
  return list;
}

{% endraw %}
{% endhighlight %}

![names](/img/posts/010/names.jpg){: .center-image }

변수를 만들었다면 다음과 같이 상품 상세보기 태그를 생성해 줍니다.

{% highlight html %}
{% raw %}

<script>
  fbq('track', 'ViewContent', {
    'content_ids': {{상품 ID 배열}},
    'content_name': {{상품 이름 배열}},
    'content_type': 'product'
  });
</script>

{% endraw %}
{% endhighlight %}

![픽셀뷰콘텐츠](/img/posts/010/pixel_viewcontent.jpg){: .center-image }

이벤트가 잘 실행되는 것을 확인할 수 있습니다.

![픽셀뷰콘텐츠이벤트](/img/posts/010/viewcontent_event.jpg){: .center-image }

<div id="3"></div>
## 3. 장바구니에 넣기

이번에는 content_ids가 아닌 contents 개체 속성을 이용해서 설정해보겠습니다.

![contents](/img/posts/010/contents.jpg){: .center-image }

id와 quantity가 필수로 사용되고 있는 것을 확인할 수 있습니다.

앞서 GA 전자상거래 설정을 할 때 productInfo라는 변수를 만들고 dataLayer에 넣었던 적이 있습니다. 형태를 보니 이 값을 그대로 사용해도 될 것 같습니다.

![addtocartcode](/img/posts/010/addtocartcode.jpg){: .center-image }

아래와 같이 변수를 만들어 줍니다.

![productinfo](/img/posts/010/productinfo.jpg){: .center-image }

새로 태그를 만들고 contents에 이 변수를 그대로 사용하면 됩니다.

상품 정보(GA4)와 상품 정보(UA)는 서로 형태가 다르니 실수하지 않도록 합니다.

![addtocarttag](/img/posts/010/addtocarttag.jpg){: .center-image }

<div id="4"></div>
## 4. 주문서 작성

참고 자료에서 주문서 작성은 어떤 표준 이벤트 이름을 사용하는지 확인합니다.

![checkout](/img/posts/010/checkout.jpg){: .center-image }

이전에 만들어둔 변수들이 있으니 태그만 바로 생성하면 완료입니다.

![checkouttag](/img/posts/010/checkouttag.jpg){: .center-image }


<div id="5"></div>
## 5. 결제 완료

결제 완료 이벤트는 currency와 value 속성이 필수로 포함되어야 합니다.

![purchase](/img/posts/010/purchase.jpg){: .center-image }

GA(UA), GA4 설정을 완료하였다면 매출액 변수가 만들어져 있을 것입니다. 없다면 이전 글을 참고하여 변수를 만들고 태그를 만들 때 아래와 같이 코드를 입력합니다.

{% highlight html %}
{% raw %}

<script>
  fbq('track', 'Purchase', {
    'contents': {{상품 정보(UA)}},
    'currency': 'KRW',
    'value': {{매출액}},    
    'content_type': 'product'
  });
</script>

{% endraw %}
{% endhighlight %}

![purchasetag](/img/posts/010/purchasetag.jpg){: .center-image }

이렇게 쇼핑몰에서의 기본적인 구매 과정에 대한 페이스북 픽셀 설정이 모두 완료되었습니다.

테스트를 통해 확인해보면 상품 상세보기부터 결제 완료까지 확인되는 것을 볼 수 있습니다.

![complete](/img/posts/010/complete.jpg){: .center-image }

추가적으로 추적하고자 하는 이벤트는 페이스북 개발자 문서를 참조하여 위와 비슷하게 설정하면 됩니다.
