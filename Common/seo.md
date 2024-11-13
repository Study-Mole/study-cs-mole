## 검색엔진 최적화(SEO)

검색엔진 최적화(Search Engine Optimization, SEO)는 검색 엔진에서 웹사이트의 가시성을 높이고 검색 결과 상위에 노출되도록 최적화하는 작업이다.
<br>더 많은 트래픽을 유도하여 방문자 수를 늘리고, 이를 통해 비즈니스의 매출이나 인지도를 높이는 목적을 갖는다.

블로그 제목과 본문에 주요 키워드를 자연스럽게 배치하여 광고를 때리고 부가 수익을 내거나, 상품 페이지에서 설명을 자세히 작성하고 사용자 리뷰를 추가해 신뢰도를 높이며 비슷한 상품을 추천하여 페이지 머물 시간을 늘리는 것과 같은 모든 활동이 **SEO**에 해당한다고 볼 수 있다.

### 최적화 방법

<br>1. 키워드 연구 (Keyword Research)

- 사용자가 많이 검색하는 키워드를 분석해 콘텐츠에 포함시킨다.
- 키워드의 검색량과 경쟁도를 고려하여 사이트와 연관된 적절한 키워드를 선정하는 것이 중요하다.

<br>2. 온페이지 최적화 (On-Page Optimization)

- 웹페이지의 콘텐츠, 제목 태그, 메타 태그, 이미지 Alt 속성 등을 이용할 수 있다.
- 검색 엔진이 쉽게 이해할 수 있도록 구조화된 콘텐츠를 제공하는 것이 핵심이다.

<br>3. 모바일 최적화 (Mobile Optimization)

- 모바일 기기에서도 사용자 경험이 원활하도록 사이트를 반응형으로 구성할 수 있다.

<br>4. 링크 구축 (Link Building)

- 다른 웹사이트에서 자신의 사이트로 연결되는 링크를 많이 확보하는 작업이다.
- 외부 사이트에서의 링크를 백링크(backlink)라고 한다. 사이트의 신뢰도를 높여준다.

<br>5. 콘텐츠 품질 개선 (Quality Content)

- 유용한 정보를 높은 퀄리티의 콘텐츠로 지속적으로 제작 및 업데이트한다.
- 매일 릴스를 하나씩 올리면 언젠가는 알고리즘을 타게 되어있다고 하는 걸 들은 것도 같다.

<br>6. 사이트 속도 최적화 (Site Speed Optimization)

- 대한민국 땅에서 긴 로딩 시간을 참을 수 있는 사람이 많지가 않다.
- 사이트가 느리면 사용자 이탈률이 높아지므로 검색 순위에도 부정적인 영향을 미친다.

<br>

### 온페이지 최적화 예시

`HTML`에서 `Semantic Tag`를 활용하는 코드를 가져와봤다.

**1. HTML 헤더 최적화**

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <!-- title 태그: 주요 키워드로 제목 구성 -->
    <title>Best Moles 2024 | Comfort & Performance</title>

    <!-- Meta Description: 페이지 요약 설명 작성. 검색 결과에 함께 표시된다. -->
    <meta
      name="description"
      content="Discover the best Moles of 2024 for comfort and performance. Read reviews, compare features, and find the perfect pair for your running needs."
    />

    <!-- Meta Keywords: 키워드를 나열한다. 현재는 잘 안쓰인다. -->
    <meta
      name="keywords"
      content="Moles, best Moles 2024, comfortable Moles, performance shoes"
    />

    <!-- Viewport Setting: 모바일 최적화 -->
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />

    <!-- Meta Open Graph: SNS 공유 시에 뜨는 미리 보기 화면 구성에 사용된다. -->
    <meta
      property="og:title"
      content="Best Moles 2024 | Comfort & Performance"
    />
    <meta
      property="og:description"
      content="Discover top-rated Moles with expert reviews and comparisons."
    />
    <meta property="og:image" content="https://example.com/images/moles.jpg" />
    <meta property="og:url" content="https://example.com/best-moles-2024" />
  </head>
  <body>
    <!-- 본문 내용 (Body Content) -->
    <h1>Best Moles of 2024</h1>
    <p>
      Looking for the best Moles to improve your computer science knowledge?
      We’ve compiled a list of top picks based on expert reviews and developer
      feedback.
    </p>
  </body>
</html>
```

**2. 이미지 최적화**

```html
<!-- alt나 title을 활용하자. -->
<img src="best-moles-2024.jpg" alt="Best Moles 2024 for CS" />
```

**3. JSON-LD 구조화 데이터 (Structured Data)**

`JSON-LD` (JavaScript Object Notation for Linked Data)는 웹에서 데이터를 표현하기 위한 형식으로, 주로 웹사이트의 메타데이터를 정의하는 데 사용된다.
<br>SEO 측면에서 웹 페이지의 내용을 어떤 종류의 정보인지 명확히 설명하여 검색 엔진에 최대한 자세한 정보를 제공하는 데 중요한 역할을 한다.

```html
<script type="application/ld+json">
  {
    "@context": "https://schema.org",
    "@type": "Article",
    "headline": "SEO에 대한 이해",
    "author": {
      "@type": "Mole",
      "name": "yubin"
    },
    "datePublished": "2024-10-27",
    "image": "https://example.com/image.jpg"
  }
</script>
```

### 마치며

구글링을 할 때, 우리가 원하는 정보는 앵간하면 첫 페이지 안에 있고, 보통 그 이상의 페이지를 열어보진 않는다.
<br>다양한 SEO 전략을 활용하여 이 넓고 깊은 정보의 바다에서 파워블로거로 거듭나 보도록 하자.

<br/>
