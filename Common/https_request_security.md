# 사용자가 페이지를 떠날 때 안정적으로 HTTP 요청 보내기

## 1. 문제 상황: 사용자가 페이지를 이동(또는 닫기)할 때 일어나는 HTTP 요청 취소

### 1.1. 단순한 예시

```html
<a href="/some-other-page" id="link">Go to Page</a>

<script>
  document.getElementById("link").addEventListener("click", (e) => {
    fetch("/log", {
      method: "POST",
      headers: {
        "Content-Type": "application/json",
      },
      body: JSON.stringify({
        some: "data",
      }),
    });
  });
</script>
```

- 이 코드는 링크를 클릭했을 때 서버로 데이터를 전송하고, 그 후 페이지 이동을 진행한다.
- 하지만 네트워크가 느리거나 요청이 제대로 완료되기 전에 페이지가 언마운트되면, 해당 요청이 취소될 수 있다.
- 즉, **서버로 로그를 보냈다고 생각했는데 실제로는 서버에 도착하지 않았을 수도 있다**는 문제가 생긴다.

> **왜 이런 문제가 발생할까?**  
> 브라우저가 페이지를 떠날 때, 진행 중인 비동기 HTTP 요청을 끝까지 보장하지 않기 때문이다. 페이지가 메모리에서 내려가는 순간 백그라운드에서 돌던 요청이 중단될 수 있다.

## 2. 왜 브라우저가 요청을 취소해버릴까?

### 2.1. 비동기 요청의 특성

- `fetch()`나 `XMLHttpRequest`는 기본적으로 **비동기**이다.
- 비동기 요청은 메인 스레드를 막지 않고, 브라우저가 백그라운드에서 관리한다.
- 페이지가 종료될 때, 브라우저는 “떠나는 페이지에 대한 요청을 굳이 유지할 필요가 없다”고 판단해 요청을 취소할 수 있다.

### 2.2. 예전 방식: 동기화 요청

- 과거에는 `XMLHttpRequest`를 `synchronous` 모드로 설정해, 요청이 끝날 때까지 **페이지 이동을 강제로 지연**시키는 방법이 있었다.
- 그러나 메인 스레드를 멈추게 만들기 때문에 성능 문제가 심각하고, 최신 브라우저(Chrome 80+)에서는 사실상 지원되지 않는다.

## 3. 시도할 수 있는 여러 가지 방법

### 3.1. 응답이 돌아올 때까지 페이지 이동을 지연시키기

```js
document.getElementById("link").addEventListener("click", async (e) => {
  e.preventDefault();

  // fetch가 완료될 때까지 대기
  await fetch("/log", {
    /* ... */
  });

  // 그 후 페이지 이동
  window.location = e.target.href;
});
```

- 이렇게 하면 **링크 클릭 → 서버에 로그 요청 전송 완료 → 그 후 페이지 이동** 순서를 강제할 수 있다.

### 문제점

1. **사용자 경험 저하**: 요청이 오래 걸리면, 사용자는 “링크가 안 눌린다”거나 “화면이 안 넘어간다”고 느낄 수 있다.
2. **탭을 닫는 상황은 막지 못함**: `e.preventDefault()`로 링크 이동은 막을 수 있지만, **탭을 닫거나 브라우저 자체를 종료**하는 것은 막을 수 없다. 따라서 여전히 요청이 취소될 가능성이 남는다.

즉, 페이지 이동은 지연시켜도 **페이지 완전 종료**를 막을 수 없으므로 신뢰도가 떨어진다.

## 4. 브라우저에 “요청은 끝까지 살려둬!”라고 지시하는 두 가지 방법

### 4.1. fetch() + keepalive 옵션

```js
fetch("/log", {
  method: "POST",
  headers: { "Content-Type": "application/json" },
  body: JSON.stringify({ some: "data" }),
  keepalive: true,
});
```

- `keepalive: true`를 지정하면, **페이지가 언마운트되어도 요청이 계속 진행**되도록 시도한다.
- 따라서 페이지 이동 중에 취소되던 상황이 크게 줄어든다.

### 4.1.1. 장점

- 기존 `fetch()` 구문에 옵션 하나만 추가하면 되므로 간단하다.
- GET/POST, 커스텀 헤더, 인증 등 다양한 HTTP 옵션을 자유롭게 사용할 수 있다.
- 이미 fetch 폴리필을 쓰는 프로젝트라면 적용 부담이 적다.

### 4.1.2. 단점

- 우선순위가 비교적 높아, 다른 리소스와 경합할 가능성이 있으나 일반적으로 크게 문제되진 않는다.
- 구형 브라우저는 `fetch` 자체를 지원하지 않을 수 있어 폴리필이 필요할 수 있다.

### 4.2. Navigator.sendBeacon()

```js
const blob = new Blob([JSON.stringify({ some: "data" })], {
  type: "application/json; charset=UTF-8",
});
navigator.sendBeacon("/log", blob);
```

- **`sendBeacon()`**은 “서버에 데이터를 최대한 안전하게 전송”하기 위한 함수이다.
- 이를 사용하면 브라우저가 **낮은 우선순위**로 전송하되, 페이지가 종료되더라도 끝까지 요청을 처리하려고 노력한다.

### 4.2.1. 장점

- **낮은 우선순위**로 전송하므로 사용자 경험을 덜 해치면서, **가능한 한** 안정적으로 전송을 완료한다.
- API가 단순하고 설정 가능한 옵션이 적어 구현이 가볍다.

### 4.2.2. 단점

- **커스텀 헤더를 자유롭게 지정하기 어렵다**는 제약이 있다. (예: `Content-Type` 커스터마이징에 제한이 있음)
- GET 방식 호출 등을 처리하려면 번거로울 수 있다.
- 구형 브라우저 호환성이 떨어질 수 있다.

## 5. ping 속성 (a 태그의 ping)

```html
<a href="http://localhost:3000/other" ping="http://localhost:3000/log">
  Go to Other Page
</a>
```

- `<a>` 태그에 `ping`을 지정하면, 링크 클릭 시 브라우저가 **자동으로 작은 POST 요청**을 보낸다.
- `ping-from`, `ping-to` 등의 헤더가 기본으로 포함되어 전송된다.

### 장점

- 별도의 자바스크립트 코드 없이, “링크 클릭 → 추적용 POST 요청”을 간단히 구현할 수 있다.

### 단점

1. 브라우저마다 지원이 고르지 않다(Firefox에서 기본적으로 꺼져 있음).
2. `<a>` 태그 클릭 외의 액션(버튼, 폼 제출 등)에는 적용하기 어렵다.
3. 커스텀 데이터를 담아 보내는 것이 사실상 쉽지 않다.

## 6. 결론: 언제 어떤 걸 사용하면 좋을까?

- **fetch() + keepalive**

  - 커스텀 헤더, POST/GET, 인증 토큰 등 **다양한 HTTP 옵션**이 필요한 경우
  - 이미 fetch 폴리필을 사용하고 있다면 도입이 간단
  - 로그 외에도 중요한(또는 복잡한) API 요청까지 다뤄야 할 때 적합

- **sendBeacon()**

  - “로그나 분석 데이터”처럼 **낮은 우선순위**로 가볍게 처리해도 무방한 단방향 전송에 적합
  - 사용자 경험을 해치지 않고, 가벼운 로그를 보내는 데 좋다
  - 다만 커스텀 헤더/메서드 제한이 있다

- **ping 속성**
  - 간단한 “클릭 추적” 용도에 적합
  - 브라우저 호환성과 기능 제한이 명확하다

## 참고 링크

- [Navigator.sendBeacon() MDN 문서](https://developer.mozilla.org/en-US/docs/Web/API/Navigator/sendBeacon)
- [fetch() MDN 문서 (keepalive 옵션)](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API)
- [ping 속성 WHATWG 스펙](https://html.spec.whatwg.org/multipage/semantics.html#ping)
