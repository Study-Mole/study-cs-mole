# React Summary

- [Life Cycle](#lifecycle)
- [React Hook Form](#react-hook-form)
- [React Query](#react-query)
- [hydration](#hydration)
- [React Memoization](#react-memoization)

- [React Suspense](#react-suspense)

- [useEffect, 오남용을 피하는 5가지](#useeffect-오남용을-피하는-5가지)


## Life cycle

**클래스형 컴포넌트**에서 `Mount`, `Update`, `Unmount`의 유형을 갖는 리액트 생명주기의 가장 큰 목적은 메모리 관리입니다. 각 생명주기 별로 작업을 위해 사용되는 메서드들이 존재하며, **함수형 컴포넌트**에서는 사용할 수 없으나 `useEffect`, `useState`, `useMemo` 등의 Hooks를 통해 유사한 동작을 구현할 수 있습니다.

## React Hook Form

React Hook Form은 비제어 컴포넌트 방식을 활용해 폼 데이터를 효율적으로 관리하고, 실시간 유효성 검사와 데이터 동기화를 지원하는 라이브러리입니다. 제어 컴포넌트 방식에 비해 리렌더링이 적어 성능이 좋고, useForm, formState, watch, clearErrors 등의 다양한 기능을 통해 폼 상태를 편하게 관리할 수 있습니다.

## React Query

React Query는 서버 상태를 효율적으로 관리하는 라이브러리로, 데이터 fetching, 캐싱, 동기화, 에러 핸들링을 자동화하여 비동기 데이터를 편리하게 다룰 수 있습니다.

## hydration

hydration은 React에서 서버 사이드 렌더링된 정적인 HTML을 클라이언트측에서 동적인 페이지로 활성화하는 과정을 말합니다. 다시말해, 빈 HTML 코드를 실행에 필요한 코드들로 채워주는 과정입니다. 서버단에서 이미 렌더링된 페이지를 넘겨주므로 CSR 보다 페이지 로딩 속도가 빠르며 SEO에도 유리합니다.

## React Memoization

React Memoization은 동일한 입력에 대해 이전 결과를 재사용하여 불필요한 리렌더링을 방지하여 성능을 최적화하는 기법입니다. `memo`는 컴포넌트를 메모이제이션하여 props가 변경되지 않으면 이전 컴포넌트를 재사용하고, `useMemo`는 계산된 값을 메모이제이션하여 의존성이 변경될 때만 다시 계산합니다.

# React Suspense

`React Suspense`는 컴포넌트의 렌더링을 일시 중지하고 데이터 로딩을 기다릴 수 있도록 하여 사용자 경험을 개선하는 기능입니다. 기존 `Fetch-on-Render` 방식은 컴포넌트가 중첩될수록 로딩이 지연되는 문제가 있지만, `Render-as-You-Fetch` 방식을 사용하면 로딩과 동시에 렌더링을 시작할 수 있습니다. Suspense는 `React.lazy`를 활용한 코드 분할이나 Tanstack Query의 `useSuspenseQuery`와 함께 사용하면 자연스러운 로딩 처리를 할 수 있으며, 에러 처리를 위해 `Error Boundary`와 조합할 수도 있습니다. 이를 통해 초기 로딩 속도를 개선하고, 스켈레톤 UI 등을 활용하여 보다 부드러운 사용자 경험을 제공할 수 있습니다.

## useEffect, 오남용을 피하는 5가지

useEffect는 비동기 로직, 외부 리소스 연결 등에 유용하지만, 남용하면 불필요한 렌더링과 복잡한 데이터 흐름 문제를 일으킬 수 있습니다. 경쟁 상태를 방지하려면 클린업을 사용하고, 이벤트 핸들러에서 상태를 효율적으로 관리해야 합니다. 또한 앱 전체 초기화 로직이나 언마운트된 컴포넌트의 상태 변경을 피하는 것이 중요합니다. 결론적으로 useEffect는 외부 리소스 초기화나 구독 해제, 컴포넌트 라이프사이클에 맞는 로직 수행에 적합합니다.

