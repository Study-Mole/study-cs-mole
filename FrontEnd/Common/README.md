# FrontEnd > Common

- [브라우저 동작 과정](#브라우저-동작-과정)
- [REST API](#rest-api)
- [앱 개발 방식](#앱-개발-방식)
- [검색엔진 최적화(SEO)](#검색엔진-최적화seo)

---

## 브라우저 동작 과정

### 브라우저란?

브라우저(Browser)는 사용자가 인터넷에 접속하여 웹 페이지를 탐색하고 HTML 문서, 이미지 등 여러 컨텐츠를 우리에게 표현해주는 소프트웨어 또는 애플리케이션을 의미한다.

Google Chrome, Apple의 Safari, Microsoft Edge, Mozilla의 FireFox 등이 있다.

주요 기능은 <u>사용자가 선택한 자원을 서버에 **요청**하고, 서버로부터 받은 **응답**을 브라우저에 **렌더링**하는 것</u>이다.

### 웹 브라우저와 웹 서버의 통신 과정

사용자가 브라우저에 웹 페이지 주소를 입력하면 웹 브라우저와 웹 서버는 아래와 같이 통신한다.

![네트워크 동작 과정](../images/network.jpg)

1. 사용자가 도메인 주소를 넣으면 DNS를 통해 도메인 주소에 해당하는 IP 주소를 얻는다.
2. IP 주소 기반으로 HTTP Request가 만들어지고, 해당 IP를 가진 서버에 전송된다.
3. 서버에서는 TCP/IP 네트워크 스택을 통해 응답을 만들어 전송한다.
4. 웹 브라우저는 패킷의 body에 들어있는 HTML 파일을 렌더링 해서 웹 브라우저에 보여준다.

### 브라우저 구조

브라우저는 아래와 같은 구조를 가진다.

![브라우저 구조](../images/browser.jpg)

- `User Interface`: 요청한 페이지를 보여주는 창 이외의 모든 UI
  - ex) 주소창, 새로고침, 북마크 등
- `Browser Engine`: UI와 렌더링 엔진의 중개자.
  - ex) 새로고침 버튼이 클릭 되었을 때 브라우저 엔진이 이를 수행
- `Rendering Engine`: HTML, CSS, JS를 파싱한 결과를 바탕으로 페이지를 그림
- `Network`: HTTP, HTTPS 같은 프로토콜을 이용해 외부 리소스를 얻음
- `JavaScript 인터프리터`: JavaScript를 해석하고 실행
- `UI 백엔드`: 렌더링 엔진이 분석한 렌더 트리를 브라우저에 그리는 역할. 브라우저가 동작하고 있는 운영체제의 인터페이스들을 따르는 기본적인 위젯 처리
  - ex) 안드로이드와 iOS의 서로 다른 Alert, Select Box 등
- `웹 스토리지`: 브라우저 자체에서 하드디스크와 같이 데이터를 로컬에 저장하기 위한 레이어

### 렌더링 동작 과정 (Critical Rendering Path)

웹 브라우저와 웹 서버의 통신이 끝나고, 브라우저에 출력되는 단계([웹 브라우저와 웹 서버의 통신의 4번째 단계](#웹-브라우저와-웹-서버의-통신-과정))를 Critical Rendering Path라고 한다. 이 단계는 크게 5단계로 구분된다.
웹 페이지에 사용자가 접속하게 되면, 네트워크를 통해 해당 페이지의 HTML 문서를 얻어올 수 있다. 그러면 렌더링 엔진이 읽어 들인 HTML 문서를 해석한다.

**1. 2. DOM Tree와 CSSOM Tree Build**
<br/>
브라우저는 HTML, CSS, JavaScript 세 종류의 언어를 해석할 수 있다. 그 중 JavaScript는 렌더링 엔진이 아니라 별도의 JavaScript 해석기라는 별도의 레이어에서 언어를 해석한다. 따라서 렌더링 엔진에서는 HTML과 CSS를 해석한다.

1. _DOM Tree Build_
   <br/>HTTP 또는 HTTPS 통신으로 byte 형태의 HTML 파일을 가져오게 된다. 이 후 이 byte 형태의 데이터를 DOM으로 전환하는 작업을 수행한다.

2. _CSSOM Tree Build_
   <br/>렌더링 엔진은 HTML 문서를 위에서부터 한 줄씩 파싱하며 DOM을 생성한다. 그러다 CSS를 로드하는 link 태그 혹은 style 태그를 만나면 DOM 생성을 중지한 후 CSS parsing의 결과물인 CSSOM을 생성하는 과정을 진행한다.

**3. 렌더 트리 구축**
<br/>DOM 트리가 구성되는 동안 브라우저는 렌더 트리를 구성한다. 렌더 트리는 DOM 트리와 CSSOM 트리를 조합하여 만들어진다. 이는 어떠한 요소들이 보여야 하는지, 어떤 스타일이 적용되어야 하는지, 그리고 어떤 순서로 나타낼 것인지를 명세한다.

**4. 레이아웃 또는 리플로우**
<br/>
이 단계에서 Rendering Tree의 각 Node들의 위치와 크기가 계산된다.<br/>페이지에서 각 객체의 정확한 위치와 크기를 계산하기 위해 브라우저는 렌더링 트리를 루트에서부터 순회한다. viewport 내에서 각 요소의 정확한 위치와 크기를 캡쳐하는 상자모델이 출력되고, 모든 상대적인 측정 값은 픽셀로 변환된다.

**5. 페인트**
<br/>
말 그대로 레이아웃 단계를 통해 화면에 배치된 엘리먼트에 색을 입히고 레이어의 위치를 결정하는 단계이다. 문서가 클수록 브라우저가 수행해야 하는 작업도 더 많아지며, 스타일이 복잡할수록 시간이 더 소요된다.

[참고]

- [프론트엔드 개발자라면 알고 있어야 할 브라우저의 동작 과정](https://yozm.wishket.com/magazine/detail/1338/)
- [Mdn, 웹페이지를 표시한다는 것: 브라우저는 어떻게 동작하는가](https://developer.mozilla.org/ko/docs/Web/Performance/How_browsers_work)
- [웹 브라우저의 동작 과정](https://velog.io/@duck-ach/Network-%EC%9B%B9-%EB%B8%8C%EB%9D%BC%EC%9A%B0%EC%A0%80%EC%9D%98-%EB%8F%99%EC%9E%91-%EA%B3%BC%EC%A0%95)

## REST API

HTTP 기반의 요청-응답 구조로 자원 관리와 상태 비저장성을 특징으로 하는 웹 서비스이다.
<br>Representational State Transfer 원칙을 따르는 모든 API를 의미한다.

### REST API vs RESTful API

`REST API`와 `RESTful API`는 자주 혼용되지만, 실제로는 조금 다른 개념이다.
<br>REST 원칙을 다소 느슨하게 적용해도 `REST API`라고 부를 수 있는 반면, `RESTful API`는 REST 원칙을 지대로 구현한 API를 지칭하는 데 더 중점을 둔다.

### REST (Representational State Transfer)

이름으로 구분되는(Representational) 자원의 상태(State)를 주고받는(Transfer) 모든 것을 의미한다.

**구성 요소**

- 자원(Resource) : HTTP URI
- 자원에 대한 행위(Verb) : HTTP Method (GET, POST, PUT, PATCH, DELETE)
- 자원에 대한 행위의 내용 (Representations) : HTTP Message Pay Load

**기본 원칙**

- Client-Server 구조

  - 클라이언트와 서버가 분리된 구조로 동작한다.
  - 클라이언트는 UI를 담당하고, 서버는 데이터를 처리하고 저장한다.

- 무상태성 (Stateless)

  - 서버는 각 요청을 독립적으로 처리하며, 요청 간에 서버에 클라이언트의 상태가 저장되지 않는다.
  - 각 요청은 필요한 모든 정보를 포함해야 한다.

- 캐시 가능 (Cacheable)

  - 응답은 캐시될 수 있고, 클라이언트는 이를 통해 서버와의 상호작용을 최소화하고 성능을 향상시킬 수 있다.

- 일관된 인터페이스 (Uniform Interface)

  - API는 일관성 있는 인터페이스를 제공해야 한다.
  - REST API에서 중요한 부분은 URI(Uniform Resource Identifier)를 사용해 자원에 접근하는 것이다.

- 계층화 (Layered System)
  - 클라이언트는 서버와 직접 통신하지 않고 중간 서버(예: 로드 밸런서, 프록시)를 통해 통신할 수 있다.

### REST API의 장점

- 확장성: 클라이언트와 서버가 독립적으로 개발 및 배포될 수 있다.

  - 서버에서 데이터를 업데이트해도, 프론트엔드는 데이터를 가져와서 화면에 표시하기만 하면 된다.
  - 반대로 화면 구조가 달라져지더라도, 서버는 그대로 데이터를 제공할 수 있다.

- 유연성: REST는 다양한 형식(JSON, XML, HTML 등)으로 데이터를 교환할 수 있다.

  - 클라이언트별로 선호하는 형식이 다를 수 있는데, 그저 동일한 API 요청 안에서 형식만 변경하면 된다.

- 명확한 자원 관리: URL을 통해 자원을 명확하게 정의하고, HTTP 메서드를 통해 자원에 대한 작업을 표현한다.
  - 쇼핑몰을 예로 들어보자. `GET /products/1`은 ID가 1인 상품 조회, `POST /products`는 새 상품 등록. `PUT /products/1`은 ID가 1인 상품의 전체 정보 수정, `DELETE /products/1`은 상품 삭제라는 점이 명확하게 드러난다.

### REST API의 단점

- 복잡한 요청 처리: 상태 비저장성을 유지하면서 복잡한 요청 간 상호작용을 관리하는 것이 어려울 수 있다.

  - 상태 비저장성이란, 서버가 클라이언트의 이전 요청 상태를 기억하지 않는다는 의미이다.
  - 예를 들어, 장바구니에 담은 물건을 결제할 떄, 서버는 사용자가 이전에 장바구니에 뭘 담았는지 기억하지 않는다.
    <br>따라서 클라이언트는 사용자 ID 등 필요한 정보를 포함하여 요청을 보내야 한다.
    <br>또한 여러 단계의 요청이 연결되어 있는 경우, 이 상태 비저장성 때문에 개발이 비효율적일 수 있다.

- 대용량 데이터 처리: HTTP 요청이 자주 발생하는 경우, 트래픽이 늘어나 성능 이슈가 발생할 수 있다.
  - 수천 개의 상품 데이터를 한 번에 불러오고, 매번 최신 상태로 유지되어야 한다면 적합하지 않을 수 있다.

### 그 외 서버 - 클라이언트 간 데이터 통신 방식

서버와 클라이언트 간의 데이터 전송을 위한 프로토콜 혹은 기술로, 다양한 상황에 맞춰 사용할 수 있다.

- TCP/IP Socket: 저수준의 네트워크 통신으로 양방향 연결을 통해 데이터 전송.
- gRPC: 빠르고 효율적인 바이너리 기반의 원격 프로시저 호출(RPC) 프로토콜, 주로 마이크로서비스 간 통신에 사용.
- WebSocket: 지속적인 양방향 통신을 지원하는 프로토콜로, 실시간 데이터 전송에 적합.

<br>

## 앱 개발 방식

### 네이티브앱

가장 보편적인 앱개발 방식으로, 스노우 앱 대부분의 화면이 네이티브로 구현되어 있다.
<br>안드로이드 OS와 iOS가 요구하는 언어인 Java, Kotlin, Swift, 그리고 Objective-C를 사용한다.

#### 네이티브앱의 장점

OS 각각의 요구 방식에 맞게 기능을 자유롭게 구현할 수 있다.
<br>다른 방식에 비해 구동 **속도가 빠르며**, 특히 푸시 메시지, 블루투스, 위치기반 서비스, QR 코드 인식, 주소록 연동, SNS 로그인, 인앱 결제 등 **다양한 모바일 기능에 쉽게 접근**할 수 있다.

#### 네이티브앱의 단점

기능 구현의 자유도가 높은 만큼, **높은 수준의 기술력**이 요구된다.
<br>상대적으로 **용량이 크고, 개발 기간과 비용이 많이 든다.**
<br>**수정이나 변경**이 발생하면 앱스토어 심사를 거쳐 **재배포해야** 하므로 절차가 번거로울 수 있다.

### 웹앱

웹 기반 앱 개발 방식을 말하며, 우리은행 등 금융사에서 이 방식을 사용한다.
<br>모바일OS가 아닌 HTML5 같은 표준 **웹 언어를 이용**하며, 브라우저를 통해 접근할 수 있다.

#### 웹앱의 장점

웹 언어로 개발하기 때문에 **기종에 구애받지 않는다.** 안드로이드 OS와 iOS 개발을 한 번에 처리할 수 있다.
<br>네이티브앱에 비해 **비용이 비교적 저렴하고 단기간 내에 제작 가능하다.**
<br>앱 수정이나 변경이 생겨도 **재배포할 필요 없이 항상 최신 버전을 유지**할 수 있다.

#### 웹앱의 단점

모바일 OS를 거치지 않다 보니 네이티브 앱과 비교해 **OS 관련 기능 접근성이 떨어진다.**
<br>별도의 **URL을 통해서만 접속 가능**하고, 로딩 속도가 상대적으로 **느리며 온라인 상태에서만** 사용할 수 있다.

### 하이브리드앱

네이티브 앱과 웹앱의 장점을 섞어놓은 넘으로 앱 안에서 웹페이지를 불러오는 방식이다.
<br>쿠팡, 크롬이 대표적이다.
<br>HTML, CSS, JavaScript 등 표준 **웹 언어로 웹페이지를 구현하고, 이를 운영체제별 앱 형태로 배포한다.**

#### 하이브리드앱의 장점

앱의 형태지만 웹 언어를 사용하여 **수정과 변경이 용이**하고, 다양한 플랫폼에 빠르게 대응할 수 있다.
<br>**모바일 OS 기능**도 활용할 수 있으며, **개발 비용과 시간이 적게 들고, 용량도 비교적 가볍다.**

#### 하이브리드앱의 단점

네이티브앱에 비해 동적 요소 구현이나 디자인 측면에서 아쉬울 수 있다.
<br>웹 언어 기반이기 때문에 **네트워크 환경에 영향을 많이 받는다.**

<br/>

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
