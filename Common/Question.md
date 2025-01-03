# Common Question

- [웹 스토리지](#웹-스토리지)
- [CORS](#cors)
- [Babel, Webpack, Polyfill](#babel-webpack-polyfill)
- [프론트엔드 디자인 패턴](#프론트엔드-디자인-패턴)

## 웹 스토리지

> **웹 스토리지의 개념과 종류에 대해 설명해주시겠어요?**

해당 도메인과 관련된 특정데이터를 웹 서버가 아닌 웹 브라우저에 저장할수 있게하는 기능입니다.
웹 스토리지의 종류로는 Local Storage와 Session Storage가 있습니다.

Local Storage는 같은 도메인 내의 모든 탭과 창에서 공유됩니다. 또한 데이터를 명시적으로 삭제하지 않는 이상 브라우저를 닫아도 데이터가 유지됩니다.

반면 Session Storage는 탭 및 창마다 독립적인 저장소를 가지고 데이터를 브라우저 세션 동안만 유지합니다. 사용자가 탭을 닫거나 창을 종료하면 데이터가 삭제됩니다.

> **쿠키와 웹 스토리지의 차이점은 무엇인가요?**

쿠키와 웹 스토리지 모두 key와 value 형태로 데이터를 저장하고 불러오고 클라이언트 측에 데이터를 저장하지만 다음과 같은 차이점이 있습니다.

첫번째로 용량의 차이가 있습니다. 쿠키는 용량이 적은반면 웹 스토리지는 쿠키에비해 아주 큰 용량을 가지고 있습니다.

두번째로 만료시점입니다. 쿠키는 특정 만료일을 지정할수있고 지정되지 않으면 지속됩니다. 웹스토리지는 LocalStorage는 영구적이고 SessionStorage는 세션동안만 유지됩니다.

세번째로는 데이터 전송입니다. 쿠키는 매번 서버에 요청할때마다 자동으로 서버로 전송되게 됩니다. 따라서 보안에 유의해야 합니다. 웹 스토리지는 서버에 자동으로 전송되지 않습니다. 따라서 클라이언트에서만 접근 가능하여서 보안성이 조금더 높습니다.

## CORS

> **CORS란 무엇인가요?**

CORS는 **Cross-Origin Resource Sharing**의 약자로, 교차 출처 리소스 공유라고도 부릅니다.

이는 한 출처(Origin A)에서 다른 출처(Origin B)의 리소스를 요청할 때 발생하는 보안정책으로,
동일 출처 정책(Same-Origin Policy, SOP)을 보완하여 제한된 범위 내에서 교차 출처 요청을 허용하는 방식입니다.

> **CORS 에러를 막기 위한 방법에는 어떤 게 있을까요?**

서버에서 Access-Control-Allow-Origin 헤더를 설정하여 요청을 허용하는 방법과 프록시 서버를 활용하여 프론트엔드와 동일한 출처에서 요청을 보내는 방법이 있습니다.

> **CORS 프로토콜은 어떤 방식으로 동작하나요?**

먼저 클라이언트가 HTTP 요청을 보낼 때, 요청 헤더에 Origin 값을 포함하여 서버에 전송합니다.
<br>서버는 응답 헤더에 Access-Control-Allow-Origin을 추가하여 허용할 출처를 명시합니다. 특정 출처만 허용하거나 \*를 사용하여 모든 출처를 허용할 수도 있습니다.
<br>브라우저는 요청의 Origin과 서버의 Access-Control-Allow-Origin 값을 비교하고, 일치하지 않으면 요청을 차단합니다.
<br>최신 브라우저는 XMLHttpRequest와 Fetch API를 통해 CORS를 사용하여 교차 출처 HTTP 요청을 처리합니다.

## Babel, Webpack, Polyfill

> **Babel과 Polyfill이 브라우저 호환성에 미치는 영향을 설명해 주세요.**

Babel과 Polyfill은 최신 JavaScript 문법과 기능을 사용하면서도 구형 브라우저와의 호환성을 확보할 수 있게 해줍니다.
<br>Babel은 ES6+ 문법을 ES5 문법으로 변환하고, Polyfill은 최신 기능을 오래된 브라우저에서도 사용할 수 있게 해주는 코드입니다. 개발자가 최신 기술을 쓰면서도 모든 사용자를 지원할 수 있게 만듭니다.

> **Webpack의 번들링 작업의 목적은 무엇인가요?**

프로젝트의 모듈화된 파일들을 번들링하여 HTTP 요청 수를 줄이고 로딩 시간을 단축시킵니다. 또한 모듈 간 의존성을 관리하고 번들 크기를 최적화하여 웹 애플리케이션 성능을 향상시키는 것이 Webpack 번들링 작업의 주된 목적입니다.

## 프론트엔드 디자인 패턴

> **프론트엔드 관점에서 디자인 패턴 활용의 장점은 무엇인가요?**

디자인 패턴은 반복적으로 발생하는 문제를 해결하기 위한 이미 검증된 설계 방식이기 때문에, 프론트엔드 개발자가 문제를 해결할 방법을 새로 설계할 필요가 없습니다. UI와 비즈니스 로직을 분리하면 각 컴포넌트의 역할이 분리되어 재사용과 유지보수가 쉬워집니다. 또한 상태 관리 패턴을 사용하면 데이터 예측이 가능합니다. 예를 들어, Flux 패턴을 따르는 Redux를 사용하면 "액션 → 리듀서 → 스토어"로 이어지는 데이터 흐름을 통해 앱 상태의 변화를 예측할 수 있습니다.
