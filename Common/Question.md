# Common Question

- [웹 스토리지](#웹-스토리지)
- [CORS](#cors)
- [Babel, Webpack, Polyfill](#babel-webpack-polyfill)
- [프론트엔드 디자인 패턴](#프론트엔드-디자인-패턴)
- [Social Login](#social-login)
- [CI/CD](#cicd)
- [캡슐화와 다형성](#캡슐화와-다형성)
- [Lighthouse](#lighthouse)
- [EM, REM, 퍼센트 차이](#em-rem-퍼센트-차이)
- [프론트엔드 아키텍쳐 & C4](#프론트엔드-아키텍쳐--c4)
- [Performance Metrics](#performance-metrix)
- [개발자도구](#개발자-도구)
- [중요 요청 체이닝 방지](#중요-요청-체이닝-방지)
- [Session과 JWT](#session과-jwt)

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

> 프로젝트의 모듈화된 파일들을 번들링하여 HTTP 요청 수를 줄이고 로딩 시간을 단축시킵니다. 또한 모듈 간 의존성을 관리하고 번들 크기를 최적화하여 웹 애플리케이션 성능을 향상시키는 것이 Webpack 번들링 작업의 주된 목적입니다.

## Social Login

> **소셜 로그인이 무엇이고, 왜 사용하는지 간단히 설명해주세요.**

가입하려는 사용자의 등록 및 로그인 절차를 간소화하여 사용자의 접근성을 높일 수 있습니다. 또한 이미 다른 플랫폼에서 검증된 사용자를 부가적인 절차없이 신뢰하는 유저로 받아들일 수 있기 때문입니다.

> **OAuth 2.0 인증 과정을 단계별로 설명해주실 수 있나요?**

1. 사용자가 소셜로그인 요청하면 클라이언트는 인증 서버로 안내
2. 안내받은 인증 서버에서 사용자가 정보 제공에 동의 및 로그인 시도
3. 로그인 성공 시 등록된 토큰을 클라이언트에 발행
4. 클라이언트는 서버에 토큰을 서버로 전달
5. 서버는 토큰이 유효한지 검사한 뒤 클라이언트에게 사용자 정보 전달
6. 클라이언트는 사용자 정보를 받아 로그인 처리

> **Access Token과 Refresh Token의 차이점은 무엇인가요? 각각의 용도에 대해 설명해주세요.**

JWT 토큰은 탈취의 위험이 있기 때문에, 유효기간이 서로 다른 Access Token과 Refresh Token을 두어 피해를 최소화 합니다. Access Token은 접근에 관여하는 토큰으로 유효기간이 짧다. Refresh Token은 재발급에만 관여하는 토큰으로 유효기간이 길다.

## CI/CD

> **프론트엔드 CI/CD 파이프라인을 구축할 때 고려해야 할 주요 요소는 무엇인가요?**

첫째, 코드 품질 관리입니다. ESLint와 Prettier로 코드 컨벤션을 자동으로 검사하고, Jest를 활용한 테스트 자동화로 안정성을 확보합니다. 이 과정은 git hook을 통해 커밋 단계에서 자동화할 수 있습니다.

둘째, 배포 전략입니다. Blue/Green 배포나 Feature Flag를 활용한 점진적 배포를 통해 서비스 안정성을 유지하면서도 신속한 배포가 가능하도록 구성합니다.

마지막으로 모니터링과 보안입니다. 에러 트래킹 도구로 실시간 에러를 모니터링하고, 의존성 취약점을 주기적으로 검사하여 안전한 서비스 운영을 보장합니다. 또한 환경 변수 관리와 접근 권한 설정도 중요한 고려사항입니다."

## 캡슐화와 다형성

> **캡슐화와 다형성의 차이점을 프론트엔드 개발 관점에서 설명해주세요**

캡슐화는 객체의 데이터를 외부에서 직접 접근하지 못하도록 보호하고, 필요한 인터페이스만 제공하는 개념입니다. React에서 `useState`의 값은 오직 `setState`라는 메서드를 통해서만 변경 가능한 캡슐화된 구조를 가지고 있습니다. 다형성은 인터페이스는 동일하지만 서로 다른 동작을 구현할 수 있도록 하는 개념입니다. 예를 들어 재사용 가능한 컴포넌트는 props에 따라 다른 모습으로 렌더링 됩니다.

> **React 컴포넌트를 개발할 때 캡슐화와 다형성을 어떻게 적용하시나요?**

데이터를 불러오는 로직을 `useFetchData`와 같은 커스텀으로 분리하여 각 컴포넌트에서는 비즈니스 로직을 신경쓰지 않도록 캡슐화할 수 있습니다. 다형성을 적용하여 `type`, `children`과 같은 속성을 props로 받아 다르게 스타일링하여 하나의 컴포넌트로 다양한 버튼을 구현할 수 있습니다.

## EM, REM, 퍼센트 차이

> **퍼센트 단위와 REM/EM 단위를 함께 사용할 때 주의해야 할 점은 무엇인가요?**

상대 단위들을 함께 사용할 때는 특히 계산에 주의해야 합니다.

EM은 부모 요소의 font-size를 기준으로 하기 때문에 중첩 사용시 의도하지 않은 크기가 될 수 있고, 퍼센트도 부모 기준이라 복잡한 레이아웃에서는 계산이 어려울 수 있습니다.
그래소 주로 폰트 크기는 루트 기준인 REM을 사용해 일관성을 유지하고, 레이아웃에는 퍼센트를 사용해 반응형을 구현하는 방식으로 각 단위의 장점을 살리는게 좋습니다.

## Lighthouse

> **프론트엔드 성능 최적화를 위한 방법에는 어떤 것들이 있을까요?**

(+ 그 중 프로젝트에서 사용했던 경험이 있다면 설명해주세요.)

리소스 최적화, 코드 스플리팅, 이미지 최적화, 캐싱 전략을 적용할 수 있습니다. 이를 통해 리소스 크기를 줄이고 필요한 코드만 로드하여 로딩 속도를 개선하며, 캐싱을 활용해 반복 요청을 최소화할 수 있습니다.

> **리액트에서 성능 최적화를 위해 사용할 수 있는 최적화 도구에 대해 아는대로 설명해주세요.**

리액트에서 성능 저하의 주요 요인은 불필요한 렌더링과 복잡한 계산 및 데이터 처리입니다. 이를 방지하기 위해 `React.memo`, `useMemo`, `useCallback`을 활용한 메모이제이션 기법으로 렌더링을 최적화할 수 있습니다. 또한, `React Profiler`와 `Lighthouse` 같은 성능 분석 도구를 사용해 병목 지점을 파악하고 개선하며, `Webpack Bundle Analyzer`를 통해 번들 크기를 최적화할 수도 있습니다.

## 프론트엔드 아키텍쳐 & C4

> **프론트엔드 아키텍처 설계 시 중요하게 고려해야 할 요소와 그 이유를 간단히 설명해주세요.**

- 라우팅: 페이지간 이동을 효율적으로 관리하고, 코드 스플리팅을 적용하여 초기 로딩 성능을 개선
- 상태 관리: 컴포넌트 간 데이터 흐름을 일관되게 유지하고, 불필요한 렌더링을 최소화
- 빌드/배포 파이프라인: CI/CD를 구축하여 자동화된 빌드, 테스트, 배포 프로세스를 설정
- 보안: SS, CSRF, CORS 등의 보안 위협을 방지하고, 민감한 데이터는 안전하게 처리
- 모니터링: 에러 및 성능 데이터를 실시간으로 수집하여 장애를 빠르게 감지하고 대응

> **C4 모델을 프론트엔드 아키텍처 설계에 적용할 때 주의해야 할 점은 무엇인가요?**

한 다이어그램에 너무 많은 정보를 담지 않아야 합니다. 또한 Level 4의 `컴포넌트`와 프론트엔드 `UI 컴포넌트` 용어가 혼동될 수 있으므로, C4의 `컴포넌트`를 `모듈`로 대체하는 것을 권장합니다.

## Performance Metrix

> **Largest Contentful Paint(LCP)가 어떤 성능 지표인지 간단히 설명해주시고, 이를 개선하기 위한 대표적인 방법 한두 가지를 말씀해 주세요.**

Largest Contentful Paint는 이미지, 비디오 등 가장 큰 콘텐츠가 렌더링 되는 시점을 의미하는 성능 지표입니다.

가장 영향이 큰 최적화 방법으로는 이미지 최적화가 있습니다.

- 이미지 파일의 저장 위치가 직접 컨트롤 할 수 있는 곳 (ex. 로컬 디렉터리)이라면 직접 이미지 크기를 줄일 수 있는 툴을 이용할 수 있습니다.
  - [자주 쓰이는 Tool: https://squoosh.app/](https://squoosh.app/)
- `jpg`, `jpeg` 보다는 `svg` 또는 `webp`과 같은 압축률이 높은 확장자를 사용하면 로딩 속도를 개선할 수 있습니다.

## 개발자도구

> **네트워크 패널을 사용해 비동기 요청(AJAX/Fetch)을 디버깅할 때 어떤 정보를 확인하고, 이를 통해 어떤 문제를 해결할 수 있나요?**

network 패널에서는 request url, method, status code, headers, body, response data 등을 확인할 수 있습니다.
이를 통해 요청이 제대로 전송되었는지(request url, method), 응답이 잘 왔는지(response data), 오류가 발생했다면 어떤 오류인지(status code) 확인할 수 있습니다.

> **Elements 패널에서 스타일 디버깅을 할 때 Computed 탭은 어떤 상황에서 주로 활용되며, 이를 통해 얻을 수 있는 이점은 무엇인가요?**

computed 탭은 요소에 적용된 최송 css를 확인할 수 있는 곳입니다. 여러 css 규칙이 덮어 씌워진 최종 스타일을 확인할 수 있습니다.
상속된 스타일과 우선순위에 따른 최종 적용값을 보여주기 때문에, 스타일이 예상한대로 적용되지 않았을 때, computed 탭을 활용하면 빠르게 원인을 파악할 수 있습니다.

## 중요 요청 체이닝 방지

> **중요 요청과, 중요 요청 체이닝에 대해서 설명해주세요.**

중요 요청은 CSS, 자바스크립트 파일 등 페이지 렌더링에 필수적인 리소스를 의미합니다. 중요 요청 체이닝은 중요한 요청들이 체인 형태로 누적되는 것입니다.

> **자바스크립트 지연에 대해서 설명해주세요.**

중요 요청이 지연되거나 로드 시간이 오래 걸리면 페이지 렌더링이 중단되는 렌더링 차단이 발생합니다. 이를 방지하기 위해, 이미지나 동영상처럼 필수적이지 않은 요소에는 defer나 async를 사용하여 자바스크립트 로드를 지연시킵니다. 이 속성들이 적용된 자바스크립트 파일은 HTML 로드 후에 실행되어 렌더링을 차단하지 않고, 페이지 로딩 성능을 최적화하며 중요 요청 체인을 짧게 유지할 수 있습니다.

## Session과 JWT

> **저장 위치, 인증 방식, 보안성, 서버 부하 측면에서 Session과 JWT의 차이점은 무엇인가요?**

Session과 JWT는 여러 측면에서 차이가 있습니다.
저장 위치 측면에서, 세션은 서버에 사용자 정보를 저장하고 클라이언트에는 세션 ID만 쿠키로 전달합니다. 반면 JWT는 사용자 정보를 토큰 자체에 인코딩하여 클라이언트에 저장합니다.

인증 방식에서, 세션은 요청마다 세션 ID를 서버가 확인하고 서버 메모리나 DB에서 사용자 정보를 조회합니다. JWT는 서명된 토큰을 검증하여 서버 조회 없이 인증이 가능합니다.

보안성 측면에서, 세션은 중요 정보가 서버에만 있어 상대적으로 안전하지만, 세션 ID 탈취 시 세션 하이재킹에 취약합니다. JWT는 서명으로 위변조를 방지하지만, 토큰 자체에 정보가 있어 탈취 시 토큰 만료 전까진 방어가 어렵습니다.

서버 부하 측면에서는 세션이 서버 메모리나 DB에 상태를 저장해 확장성에 제약이 있는 반면, JWT는 stateless하여 서버 확장에 유리합니다.

> **JWT를 사용할 때 보안 취약점을 방지하는 방법은 무엇인가요?**

JWT 사용 시 보안 취약점을 방지하는 방법은 다음과 같습니다.

첫째, 토큰 만료 시간(expiration time)을 짧게 설정하고, 리프레시 토큰을 활용하여 긴 세션을 유지합니다. 이렇게 하면 토큰이 탈취되어도 피해를 최소화할 수 있습니다.

둘째, 적절한 서명 알고리즘을 사용합니다. HS256보다는 RS256과 같은 비대칭 알고리즘이 더 안전합니다. 'none' 알고리즘은 절대 허용하지 않습니다.

셋째, 토큰에 중요한 개인정보를 담지 않습니다. JWT는 인코딩만 될 뿐 암호화되지 않으므로 민감한 정보는 포함하지 않아야 합니다.

넷째, 강력한 비밀키를 사용하고 정기적으로 교체합니다. 키가 유출되면 모든 토큰이 위험해질 수 있습니다.

다섯째, 필요하다면 토큰 블랙리스트를 관리하여 로그아웃된 토큰이 재사용되지 않도록 합니다.

마지막으로, HTTPS를 사용하여 전송 과정에서 토큰이 탈취되지 않도록 보호합니다.
