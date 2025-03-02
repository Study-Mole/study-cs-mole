# React Question

- [React Hook Form](#react-hook-form)
- [React Query](#react-query)
- [hydration](#hydration)
- [React Memoization](#react-memoization)

- [React Suspense](#react-suspense)


## React Hook Form

> **React Hook Form을 사용하는 이유는 무엇인가요?**

React의 상태와 실시간으로 연결되지 않는 비제어 컴포넌트를 사용하며 리렌더링 문제를 해결할 수 있으며, 실시간으로 유효성 검사, 데이터 동기화 등의 기능이 필요한 Form을 효과적으로 관리할 수 있기 때문입니다.

## React Query

> **상태 관리 라이브러리를 사용하는 이유는 무엇인가요?**

프로젝트의 규모가 커질수록 복잡한 구조와 계층이 생기기 시작하고, 상태를 공유하기 위해서 props를 사용한다면 과도한 props drilling 문제를 겪을 수 있습니다. 이를 방지하기 위해서 state를 전역으로 사용할 수 있는 상태 관리 라이브러리가 필요합니다.

> **React Query의 장점에 대해 설명해 주시겠어요?**

서버의 데이터를 불러오고, 캐시하고, 최신 상태로 유지하는 작업을 처리해주기 때문에 중복된 API의 호출을 방지할 수 있고 개발자는 간편하게 데이터를 관리할 수 있기 때문입니다.

## hydration

> **SPA, CSR, SSR의 차이점에 대해 설명해주세요**

SPA (Single Page Application)는 하나의 HTML 파일로 동작하며, 페이지 이동 시 JavaScript를 통해 필요한 부분만 업데이트합니다. 이 덕분에 페이지 전환이 빠르고 사용자 경험이 좋지만, 초기 로딩 속도가 느리다는 단점이 있습니다.
CSR (Client-Side Rendering)은 빈 HTML이 처음 로드되고, 이후 JavaScript가 실행되면서 화면을 그리는 방식입니다.서버 부담이 적고 SPA에 적합하지만, 초기 로딩이 느리며 검색 엔진 최적화(SEO)가 어렵다는 단점이 있습니다.
SSR (Server-Side Rendering)은 서버에서 HTML을 미리 완성해서 클라이언트에 전달하는 방식입니다.
초기 화면이 빠르게 표시되고, 검색 엔진이 HTML을 바로 읽을 수 있기 때문에 SEO에 유리합니다. 하지만, 서버 부담이 증가하고 클라이언트에서 React를 연결하는 Hydration 과정이 필요합니다.

> **SSR 렌더링이 SEO에 유리한 이유는 무엇인가요?**

SSR이 SEO에 유리한 이유는 서버에서 미리 HTML을 완성해서 제공하기 때문입니다. 검색 엔진 크롤러는 HTML만 읽을 수 있는데, SSR은 서버가 완성된 HTML을 보내기 때문에 크롤러가 페이지의 내용을 바로 인식할수 있습니다.

## React Memoization

> **React.memo와 useMemo의 차이점은 무엇인가요?**


## React Suspense

> **React Suspense를 사용하면 기존의 useEffect 기반 데이터 패칭과 비교하여 어떤 장점이 있나요?**

React Suspense는 데이터를 불러오는 동안 자동으로 로딩 상태를 관리하고, 에러 처리를 Error Boundary와 결합해 간단하게 구현할 수 있습니다. 또한 선언적 데이터 패칭 방식을 사용해 useEffect로 직접 상태를 관리하는 방식보다 코드 구조가 단순해지고, Concurrent Mode와 결합했을 때 성능 최적화에도 유리합니다.

## useEffect, 오남용을 피하는 5가지

> **useEffect를 사용하면서 발생할 수 있는 경쟁 상태 문제를 해결하는 방법에 대해 설명해주세요.**

경쟁 상태는 하나의 자원을 두 곳에서 조작하려고 할 때 조작 순서가 어긋나는 버그를 의미합니다. useEffect의 클린업 함수 내에 현재 이펙트의 유효성을 체크할 수 있는 플래그를 두어, 비동기 요청이 완료된 후 이 플래그를 true로 설정합니다. 이렇게 하면 다음 새로운 이펙트가 실행될 때 이전 요청의 결과가 상태에 영향을 미치지 않도록 방지할 수 있습니다.

> **useEffect를 잘못 사용하면 발생할 수 있는 성능 저하나 불필요한 렌더링 문제에 대해 설명하고, 이를 피하기 위한 방법에 대해 말해주세요.**

예를 자식 컴포넌트에서 useEffect를 사용해 fetch/mutate한 결과를 부모에 전달하는 구조에서는 데이터 흐름이 역방향으로 진행되어 어느 시점에 부모가 업데이트 되는지 추적하기 어렵습니다. 또한 자식 컴포넌트에서 fetch가 진행 될 때마다 부모 컴포넌트에서 불필요한 리렌더링이 발생합니다. 이런 문제를 피하기 위해 일반적으로는 부모 컴포넌트에서 데이터를 fetch하고, 자식에게 props로 전달하는 구조를 사용하는 것이 더 명확하고 유지보수 하기 쉽습니다.
