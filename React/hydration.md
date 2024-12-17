# hydration

`hydration`은 React에서 서버 사이드 렌더링(SSR)된 HTML을 클라이언트에서 동적으로 활성화하는 과정이다.
<br>즉, 서버에서 한 번 렌더링된 정적 HTML이, hydration을 거치며 interactive한 페이지로 바뀌는 것이다.
<br>이 과정은 SSR에서만 발생하는데, CSR 환경에는 React가 처음부터 클라이언트에서 모든 것을 렌더링하기 때문에 hydration을 거칠 필요가 없다.

## 좀 더 자세히

React에서는 CSR과 SSR을 결합하여 더 효율적인 페이지 로딩을 구현한다.
<br>SSR을 통해 서버에서 HTML을 렌더링하여 클라이언트로 전달하면 사용자는 처음에 정적 HTML을 보게 된다.
<br>그 때 클라이언트 측에서 정적 HTML을 React 컴포넌트로 **전환**함으로써 상태 업데이트나 이벤트 처리가 가능하게 만든다.
<br>여기서 이러한 **전환**을 처리하는 과정이 `hydration`이다.

## hydration 과정

1. 서버 사이드 렌더링 (SSR)
   서버는 React 컴포넌트를 렌더링하여 완성된 HTML을 클라이언트에게 보낸다.
   <br>이때 서버에서 렌더링된 HTML은 정적이고 동적이지 않은 상태이다.
   <br>SSR된 HTML이 브라우저에서 먼저 표시된다.

2. hydration
   클라이언트가 서버에서 받은 HTML을 로드한 후, React는 이 HTML을 카만 두고 클라이언트에서 JavaScript를 실행하여 해당 HTML을 동적으로 활성화한다.
   <br>React는 서버에서 보낸 HTML과 클라이언트 측의 React 애플리케이션 상태를 일치시켜 렌더링된 DOM을 동적으로 관리한다.

## 예를 들어

서버에서 생성된 HTML이 아래와 같을 때,

```html
<div id="root">
  <button>Click me!</button>
</div>
```

클라이언트에서 React가 이 HTML에 연결하여 이벤트를 활성화하는 과정을 거쳐

```js
const App = () => {
  const handleClick = () => alert("Clicked!");
  return <button onClick={handleClick}>Click me!</button>;
};

hydrateRoot(document.getElementById("root"), <App />);
```

코드 속 우리가 의도한 button이 사용자에게 보여지게 된다.

## hydartion 메서드

### ReactDOM.hydrate()

`ReactDOM.hydrate()`는 서버에서 전달된 HTML을 기반으로 React 애플리케이션을 클라이언트 측에서 동적으로 활성화하는 메서드이다.
<br>서버에서 보낸 HTML은 정적이므로, React는 이 HTML을 받아서 동적으로 업데이트 가능한 상태로 만들기 위해 `hydrate()` 메서드를 사용한다.

```js
ReactDOM.hydrate(<App />, document.getElementById("root"));
```

거런데 이 메서드는 React 18 이전까지 주로 사용되던 방법이다.

### hydrateRoot()

React 18부터 새로 도입된 `hydrateRoot`는 더 세밀한 동적 렌더링을 가능하게 하고,
<br>Concurrent Mode와 호환되며, React의 createRoot와 결합되어 더 나은 성능과 안정성을 제공한다.

```js
import { hydrateRoot } from "react-dom/client";

hydrateRoot(document.getElementById("root"), <App />);
```

참고로 React 18부터 도입된
<br>`Concurrent Mode`는 기존 동기적이었던 React 렌더링 방식을 비동기적으로 만드는 새로운 기능이고,
<br>`createRoot`는 ReactDOM.render() 메서드의 상위 버전으로 Concurrent Mode와 자동 배치를 지원한다고 한다.

## hydration vs CSR

### CSR

페이지가 처음 로드될 때, 서버에서 HTML을 전송하는 대신, 빈 HTML 페이지만 전송하고 클라이언트 측에서 JavaScript가 실행되어 페이지를 렌더링한다.
<br>때문에 페이지가 완전히 로드되기까지 시간이 걸리며, 초기 페이지 표시 속도가 느려질 수 있다.

### SSR + Hydration

빈 HTML이 아닌 서버에서 렌더링된 HTML을 페이지에 빠르게 표시한다.
<br>고 다음, hydration을 통해 서버로부터 받은 정적 HTML을 동적으로 활성화하므로 페이지 로딩 속도가 빠르고,
<br서버에서 렌더링된 HTML을 검색 엔진이 크롤링할 수 있기에 SEO에도 유리하다.

## 주의할 점

hydration이 지연되면 자바스크립트가 로드되고 실행될 때까지 초기에 페이지 상호작용이 제한적일 수 있다.
<br>SSR된 HTML과 클라이언트에서 React가 관리하는 DOM이 일치하지 않으면 문제가 생길 수 있다.

## 정리

React에서 Hydration은 SSR된 HTML을 클라이언트 측에서 동적으로 활성화하는 과정이다.
<br>덕분에 빠른 초기 로딩과 클라이언트 측 상호작용을 동시에 처리할 수 있으나, 문제 예방을 위해 서버와 클라이언트 간의 HTML 일치를 보장하는 것이 중요하겠다.
