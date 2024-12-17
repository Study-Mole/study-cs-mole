# React Summary

- [Life Cycle](#lifecycle)
- [React Hook Form](#react-hook-form)
- [React Query](#react-query)
- [React Memoization](#react-memoization)

## Life cycle

**클래스형 컴포넌트**에서 `Mount`, `Update`, `Unmount`의 유형을 갖는 리액트 생명주기의 가장 큰 목적은 메모리 관리입니다. 각 생명주기 별로 작업을 위해 사용되는 메서드들이 존재하며, **함수형 컴포넌트**에서는 사용할 수 없으나 `useEffect`, `useState`, `useMemo` 등의 Hooks를 통해 유사한 동작을 구현할 수 있습니다.

## React Hook Form

React Hook Form은 비제어 컴포넌트 방식을 활용해 폼 데이터를 효율적으로 관리하고, 실시간 유효성 검사와 데이터 동기화를 지원하는 라이브러리입니다. 제어 컴포넌트 방식에 비해 리렌더링이 적어 성능이 좋고, useForm, formState, watch, clearErrors 등의 다양한 기능을 통해 폼 상태를 편하게 관리할 수 있습니다.

## React Query

React Query는 서버 상태를 효율적으로 관리하는 라이브러리로, 데이터 fetching, 캐싱, 동기화, 에러 핸들링을 자동화하여 비동기 데이터를 편리하게 다룰 수 있습니다.

# React Memoization

React Memoization은 동일한 입력에 대해 이전 결과를 재사용하여 불필요한 리렌더링을 방지하여 성능을 최적화하는 기법입니다. `memo`는 컴포넌트를 메모이제이션하여 props가 변경되지 않으면 이전 컴포넌트를 재사용하고, `useMemo`는 계산된 값을 메모이제이션하여 의존성이 변경될 때만 다시 계산합니다.
