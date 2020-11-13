---
layout: post
permalink: /structured-data-blog-seo/
title: '블로그 SEO 개선, 구조화된 데이터 추가하기'
date: 2020-11-13 20:13:00 +09:00
feature: '/img/posts/003/structured-data-seo.jpg'
background: '/img/posts/003/structured-data-bg.jpg'
categories:
  - marketing
tags:
  - 마케팅
  - 디지털마케팅
  - SEO
  - 구글
  - 구조화된데이터
description: '검색엔진 최적화(SEO) 작업을 위해 구조화된 데이터를 사이트에 추가하는 방법, 블로그를 예시로 설명해드립니다.'
---

## 내 사이트를 돋보이게 하려면?

마케팅에 있어서 **검색엔진 최적화(SEO)**의 중요성은 제가 굳이 설명하지 않아도 아실 거라 생각됩니다. 검색 결과에 나 혹은 우리 회사의 글이 상위에 노출되어 있다면 광고비용을 들이지 않고도 마케팅 성과를 낼 수 있기 때문에 SEO는 가장 효과적인 마케팅 기법 중 하나라고 할 수 있습니다.

### 보다 구체적인 정보를 구글 검색 결과에 보여주고 싶다면?

구글에서 '제주도 호텔'이라는 키워드로 검색을 해보겠습니다.

![제주도 호텔1](/img/posts/003/jeju-hotel-search-1.jpg){: .center-image }

![제주도 호텔2](/img/posts/003/jeju-hotel-search-2.jpg){: .center-image }

단순히 제목과 설명만 보이는 위 사진과는 다르게 아래에는 FAQ 내용이나 숙박 요금 등 정보가 보다 다양하고 구체적으로 표현되어 있습니다. 구글 검색엔진이 어떻게 이런 것까지 보여주는 것일까요? '내 사이트도 이렇게 구조화된 데이터를 보여줌으로써 SEO를 개선할 수 있다면 좋을 텐데'라는 생각을 하시는 분들도 계실 겁니다. 이런 사이트에 더 눈길이 가고 클릭할 확률이 높아지기 때문이죠.<br><br>

> <span style="color:#0404B4;">구조화된 데이터란?</span>
>> <span style="color:#0080FF;">다양한 정보를 담고 있는 콘텐츠를 논리적으로 조직화하여 가공된 데이터를 말합니다. 검색엔진 입장에서는 콘텐츠가 조직화된 구조로 되어있다고 해석하는 데에 도움이 됩니다.</span>

![SEO](/img/posts/003/structured-data-seo.jpg){: .center-image }

## 구조화된 데이터 추가 방법

구글에서는 검색 개발자 사이트를 통해 구조화된 데이터가 검색 결과에 표시되려면 어떻게 해야 하는지 가이드를 제공해 주고 있습니다. 아래 사이트에서 참조 탭을 통해 구조화된 데이터 항목을 확인할 수 있습니다.

[구글 검색 개발자 사이트 바로 가기](https://developers.google.com/search)

추가하는 방법을 제 블로그를 예시로 설명드리겠습니다. 운영하시는 사이트가 블로그가 아니고 쇼핑몰, 애플리케이션 등 다른 유형의 사이트라면 해당 콘텐츠의 성격에 따라 알맞은 타입을 선택하여 적용하시면 됩니다.

이 블로그는 Jekyll의 clean blog 테마를 사용하고 있는데 기본적으로는 구조화된 데이터가 적용되어 있지 않기 때문에 가이드에 따라 한번 추가해보겠습니다.

`Article` 타입의 설명을 보겠습니다.

![Article 타입](/img/posts/003/structured-article.jpg){: .center-image }

뉴스 기사나 블로그 페이지 등에 `Article` 데이터를 추가하면 구글 검색 결과에 표시될 가능성이 높아진다고 합니다. AMP(Accelerated Mobile Pages)를 사용하고 있지 않기 때문에 비 AMP 웹페이지의 설명을 따라가겠습니다.

``` html

  <html>
    <head>
      <title>Article headline</title>
      <script type="application/ld+json">
      {
        "@context": "https://schema.org",
        "@type": "NewsArticle",
        "headline": "Article headline",
        "image": [
          "https://example.com/photos/1x1/photo.jpg",
          "https://example.com/photos/4x3/photo.jpg",
          "https://example.com/photos/16x9/photo.jpg"
         ],
        "datePublished": "2015-02-05T08:00:00+08:00",
        "dateModified": "2015-02-05T09:20:00+08:00"
      }
      </script>
    </head>
    <body>
    </body>
  </html>

```

설명의 예시를 보면 위와 같은 코드를 입력하도록 되어있습니다.
이런 코드를 작성해야 되는구나 하고 넘어가 보겠습니다.

구조화된 `Article` 데이터를 추가할 때 작성하게 되는 속성 값들이 있는데 AMP 페이지는 필수로 작성해야 하는 속성이 있지만 비 AMP 페이지는 권장 속성만 포함되어 있습니다.

`Article` 데이터는 `Article`, `NewsArticle`, `BlogPosting` 타입 중 하나를 기반으로 해야 하며, `dateModified`, `datePublished`, `headline`, `image` 등의 속성이 권장 속성입니다.

![property](/img/posts/003/property.jpg){: .center-image }

속성 값에 대한 자세한 설명을 확인하고 싶다면 schema.org 페이지를 참고하시면 됩니다.

[schema.org 바로 가기](https://schema.org/){: .center-image }

## schema.org란?

Google, Microsoft, Yahoo 및 Yandex와 같은 세계 유명 검색엔진들이 모여서 구조화된 데이터에 대한 스키마를 생성, 유지 관리하기 위해 schema.org를 설립하였습니다.

쉽게 말해서 **데이터 작업을 할 때 우리 모두 이러이러한 언어 형식을 사용하자**고 약속을 한 것입니다.

`Microdata`와 `RDFa`, `JSON-LD`의 세 가지 언어 형식을 지원하는데 차이점은 대략 아래와 같습니다. 여기에서는 세부적으로 살펴볼 필요는 없고 형태가 이렇구나 하고 넘어가면 될 것 같습니다.

``` html

  // Microdata 형식
  <p itemscope itemprop="organization" itemtype="https://schema.org/Organization">
    <a href="http://npr.org" itemprop="url">
    <span itemprop="name">National Public Radio</span></a> has a sponsor:
    <span itemprop="sponsor" itemscope itemtype="https://schema.org/Organization">
      <a itemprop="url" href="http://www.example.com/GloboCorp">
      <span itemprop="name">GloboCorp</span></a>
    </span>.
  </p>

```

``` html

  // RDFa 형식
  <p vocab="https://schema.org/" typeof="Organization">
    <a href="http://npr.org" property="url">
    <span property="name">National Public Radio</span></a> has a sponsor,
    <span property="sponsor" typeof="https://schema.org/Organization">
      <a property="url" href="http://www.example.com/GloboCorp">
      <span property="name">GloboCorp</span></a>
    </span>.
  </p>

```

``` html

  // JSON-LD 형식 (*권장*)
  <script type="application/ld+json">
  {
    "@context": "https://schema.org/",
    "@type": "Organization",
    "name": "National Public Radio",
    "url": "http://npr.org",
    "sponsor":
    {
      "@type": "Organization",
      "name": "GloboCorp",
      "url": "http://www.example.com/"
    }
  }
  </script>

```

구글은 `JSON-LD` 형식을 권장하기 때문에 가급적이면 권장 사항을 따르는 것이 좋습니다. 제가 작업했던 내용이 `JSON-LD` 형식입니다.

### 다시 돌아와서...

여기가 실제로 작업했던 내용입니다.
아래와 같은 코드를 head 태그 내부에 작성해 주었습니다.

```html
{% raw %}
  {% if page.feature %}
    <script type="application/ld+json">
    {
      "@context": "https://schema.org",
      "@type": "BlogPosting",
      "headline": "{{ page.title }}",
      "image": [
        "{{ site.github.url }}{{ page.feature }}"
       ],
      "datePublished": "{{ page.date }}"
    }
    </script>
  {% endif %}
{% endraw %}
```

Liquid 문법을 사용하여 썸네일이 있는 게시글에 스크립트가 적용되도록 하였습니다. 이 부분은 각자 사이트의 환경에 맞게 수정하면 됩니다.

* @context : 건드리지 않습니다.
* @type : 블로그 포스팅 글에 적용할 것이기 때문에 BlogPosting을 입력합니다.
* headline : 게시글의 제목을 작성해 줍니다.
* image : 원하시는 이미지를 나열하면 되는데 저는 썸네일 하나만 입력하였습니다.
* datePublished : 게시글이 작성된 날짜와 시간을 입력합니다.
* dateModified : 게시글이 수정된 날짜와 시간을 입력하면 되는데 굳이 입력하지 않았습니다.

이렇게 코드를 작성하고 블로그의 게시글에서 페이지 소스 보기로 확인해보면 아래와 같은 스트립트가 새로 추가된 것을 확인할 수 있습니다.

![property](/img/posts/003/source-code.jpg){: .center-image }

확인되었다면 블로그에 구조화된 데이터 추가가 완료된 것입니다.

제대로 적용되었는지 확인해보고 싶으시다면 구글 리치 검색 결과 테스트 사이트에서 URL을 입력하여 확인 가능합니다.

[리치 검색 결과 테스트 바로가기](https://search.google.com/test/rich-results)

잘 작성하였다면 아래와 같이 리치 검색 결과를 생성할 수 있다는 메시지를 확인할 수 있습니다.

![구조화된 데이터 추가 전](/img/posts/003/structured-data-before.jpg){: .center-image }

![구조화된 데이터 추가 후](/img/posts/003/structured-data-after.jpg){: .center-image }


개발 관련 지식이 필요할 수도 있고, 굉장히 복잡해 보이지만 실제로 해보신다면 생각보다 어려운 작업은 아니라는 것을 알 수 있을 것입니다. 가이드에서 알려주는 예시 코드 혹은 잘 만들어진 사이트에 들어가셔서 페이지 소스 보기로 코드를 참고하셔서 자신의 웹사이트에 맞게 활용하시는 것이 좋습니다.

구조화된 데이터를 추가한다고 해서 검색엔진의 상위 순위에 사이트가 노출된다고 보장할 수는 없습니다. SEO는 한순간에 완성되는 것이 아니고 길게는 몇 달이 걸릴 수도 있기 때문입니다. 위와 같은 작업을 한다고 무조건 구조화된 형식으로 검색 결과를 보여주는 것도 아닙니다. <span style="color:#0404B4;">"우리 사이트는 이렇게 구성되어 있어"</span>라고 검색엔진에게 정보를 제공해 줌으로써 검색 결과가 보다 구체적으로 보일 수 있도록 도와주는 것이라고 생각하면 될 것 같습니다.
