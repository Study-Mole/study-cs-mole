# React Question

- [React Hook Form](#react-hook-form)
- [React Query](#react-query)
- [hydration](#hydration)
- [React Memoization](#react-memoization)

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
