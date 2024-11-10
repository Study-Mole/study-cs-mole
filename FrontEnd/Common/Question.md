## CORS란 무엇인가요?

CORS는 **Cross-Origin Resource Sharing**의 약자로, 교차 출처 리소스 공유라고도 부릅니다.

이는 한 출처(Origin A)에서 다른 출처(Origin B)의 리소스를 요청할 때 발생하는 보안정책으로,
동일 출처 정책(Same-Origin Policy, SOP)을 보완하여 제한된 범위 내에서 교차 출처 요청을 허용하는 방식입니다

## CORS 에러를 막기 위한 방법에는 어떤 게 있을까요?

서버에서 Access-Control-Allow-Origin 헤더를 설정하여 요청을 허용하는 방법과 프록시 서버를 활용하여 프론트엔드와 동일한 출처에서 요청을 보내는 방법이 있습니다.

## CORS 프로토콜은 어떤 방식으로 동작하나요?

먼저 클라이언트가 HTTP 요청을 보낼 때, Origin 헤더에 클라이언트의 출처 URL을 담아 서버로 전송합니다 이때 서버는 응답 헤더에 Access-Control-Allow-Origin 헤더를 포함해 요청을 승일할 출처를 지정합니다.
그러면 브라우저는 클라이엍느의 요청 Origin과 서버의 Access-Control-Allow-Origin 값을 비교하여 요청을 차단합니다.

최신 브라우저는 XMLHttpRequest 나 Fetch API 에서 CORS를 사용하여 교차출처 HTTP요청을 관리합니다.

2. **서버의 응답**

   서버는 응답 헤더에 `Access-Control-Allow-Origin` 헤더를 포함해, 요청을 승인할 출처를 지정한다. 특정 출처만 허용하거나, `*`(와일드카드)를 사용하여 모든 출처를 허용할 수도 있다.

3. **브라우저의 확인**

   브라우저는 클라이언트의 요청 `Origin`과 서버의 `Access-Control-Allow-Origin` 값을 비교하여, 일치하지 않으면 요청을 차단하고 응답을 버린다.

4. **브라우저에서 지원하는 CORS API**

   최신 브라우저는 `XMLHttpRequest`나 `Fetch API`에서 CORS를 사용하여 교차 출처 HTTP 요청을 관리합니다
