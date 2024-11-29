> ### React Query의 구조가 아닌 기능 및 사용법을 보고싶다면 기능 및 사용법 목차부터 읽으시면 됩니다.

- [React Query](#react-query)
- [기능 및 사용법](#react-query의-주요-특징-및-사용법)

## React Query

### React Query를 왜 사용할까?

1. **서버 상태 관리의 간소화**: 서버 데이터를 불러오고, 캐시하고, 최신 상태로 유지하는 작업을 자동으로 처리해주어 개발자는 비즈니스 로직에 집중할 수 있다.

2. **중복 API 호출 방지**: 동일한 데이터를 여러 컴포넌트에서 요청할 때 캐시를 활용해 중복된 네트워크 요청을 줄여 성능을 최적화한다.

3. **자동 데이터 갱신**: 화면 포커스 전환, 네트워크 상태 변화 등 다양한 이벤트에 따라 데이터를 최신 상태로 자동 업데이트하여 사용자에게 최신 정보를 제공한다.

이를 통해 복잡한 서버 데이터 요청과 관리 로직을 간결하게 하고, 성능과 사용자 경험을 동시에 향상시킬 수 있다.

### ReactQuery의 상태관리

React Query는 전통적인 상태관리 라이브러리와는 조금 다른 방식으로 동작하는 비동기 데이터 관리 라이브러리이다.
서버 상태(server state) 관리를 주된 목적으로 설계되었으며, 이를 통해 클라이언트 애플리케이션에서 서버 데이터의 로딩, 캐싱, 동기화 및 업데이트를 효율적으로 처리할 수 있다.

React Query는 서버 상태(server state)에 중점을 둡니다. 서버 상태는 주로 API를 통해 가져오는 데이터로,
다음과 같은 특징이 있다.

- 비동기적으로 데이터를 가져와야 함.
- 여러 컴포넌트에서 재사용될 가능성이 높음.
- 데이터가 시간이 지나면서 변할 수 있음(캐싱 및 동기화 필요).

### React Query 라이브러리의 내부구조

핵심 기능을 처리하는 **`@tanstack/query-core`** 패키지와 이것을 리액트 프로젝트에서 사용할 수 있도록 돕는 **`@tanstack/react-query`** 패키지가 존재한다.

- **@tanstack/query-core**: 데이터를 관리하는 부분. 이 부분이 "데이터를 어디에 저장하고, 어떻게 관리할지"를 결정한다.
- **@tanstack/react-query**: 이 부분은 React와 연결하는 역할. 데이터가 바뀌었을 때 React 컴포넌트가 자동으로 새로고침 될 수 있도록 한다.

```jsx
query-core/src
├── notifyManager.ts
├── query.ts
├── queryCache.ts
├── queryClient.ts
└── queryObserver.ts

react-query/src
├── useBaseQuery.ts
└── useQuery.ts
```

추상적으로 표현한 구조의 모습은 다음과 같다.

![alt text](.//Images/react-query-1.png)

### QueryClient

```jsx
export class QueryClient {
  private queryCache: QueryCache

  constructor(config: QueryClientConfig = {}) {
    this.queryCache = config.queryCache || new QueryCache()
  }

  setQueryData() {} // 데이터를 캐시에 저장한다.
  getQueryData() {} // 캐시에서 데이터를 가져온다.
  invalidateQueries() {} // 데이터를 새로 고침할 필요가 있는지 확인하고, 새로 요청한다.
  getQueryCache() {} // queryCache 에 저장된 데이터에 접근할수 있게한다.
  // ...
}
```

`QueryCache`는 실제로 데이터를 저장하고 관리하는 저장소이다.

`QueryClient`는 `queryCache`를 가지며 이 캐시 인스턴스와 상호작용할 수 있는 몇 가지 메서드를 제공한다.

### **QueryCache**

```jsx
export class QueryCache extends Subscribable<> {
  private queries: Query<any, any, any, any>[]
  private queriesMap: QueryHashMap

  constructor(config?: QueryCacheConfig) {
    this.queries = [] // Query 목록을 저장한다.
    this.queriesMap = {} //각 Query를 찾기 쉽게 한다.
  }

  build() {}
  add() {}
  remove() {}
  // ...
}
```

`QueryCache`는 여러 개의 `Query` 인스턴스들을 자신의 내부 프로퍼티 필드인 `queries`, `queriesMap`에 등록하여 관리하는 역할을 한다.

일반적으로 개발자는 `QueryClient` 인스턴스에 접근하여 캐시와 상호작용할 수 있다.

즉 캐시에 직접 접근하여 상호작용할 필요가 없다.

- Query 는 서버에서 데이터를 가져오는 하나의 요청을 의미한다.

### **QueryCache가 제공하는 build 메서드**

```jsx
export class QueryCache extends Subscribable<> {
  private queries: Query<any, any, any, any>[]
  private queriesMap: QueryHashMap

  constructor(config?: QueryCacheConfig) {}

  build(client, options, state?) {
    const queryKey = options.queryKey!
    const queryHash = options.queryHash ?? hashQueryKeyByOptions(queryKey, options)
    let query = this.get(queryHash)
    if (!query) {
      query = new Query({
        cache: this,
        logger: client.getLogger(),
        queryKey,
        queryHash,
        options: client.defaultQueryOptions(options),
        state,
        defaultOptions: client.getQueryDefaults(queryKey),
      })
      this.add(query)
    }
    return query
  }

  add(query) {
    if (!this.queriesMap[query.queryHash]) {
      this.queriesMap[query.queryHash] = query
      this.queries.push(query)
    }
  }
}
```

### `QueryCache`의 `build`

- 옵션 객체에 주어진 **`queryKey`** 데이터를 해시한다.(with [React Query의 해시 알고리즘](https://github.com/TanStack/query/blob/main/packages/query-core/src/utils.ts#L269-L280))
- **queryHash**를 키값으로 활용하여 **queriesMap**에 저장된 **Query** 인스턴스를 조회해본다.
- **Query** 인스턴스가 존재한다면 바로 반환하고 존재하지 않는다면 새로 생성해서 캐시에 등록 후 반환한다.

> 이 동작은 각각의 리액트 컴포넌트에서 동일한 옵션으로 useQuery훅을 호출한 경우에 중요한 역할을 하게된다. queryKey를 식별자로 활용하여 캐시에 동일한 Query가 존재하고 Query가 가진 QueryState(서버의 데이터)가 유효한 경우 이 값을 재활용하게 된다. 이를 통해 중복된 네트워크 요청이 여러번 발생하지 않도록 한다.

정리하자면

이 함수의 핵심은 **기존에 같은 요청이 있다면 재사용하고, 없으면 새로 만든다.**

1. `queryKey`로 요청의 고유 ID를 정한다.
2. `queryHash`로 요청을 구별한다.
3. `queryHash`에 맞는 `Query`가 이미 있다면 그걸 반환하고, 없다면 새 `Query`를 만들어 `add`로 추가한다.

### **Query**

데이터를 서버에서 가져오는 각각의 요청을 관리한다.

```jsx
export class Query extends Removable {
  queryKey // Query의 고유한 키
  queryHash // 데이터를 찾기 쉽게 해시로 만든 고유 식별자
  options!: QueryOptions<TQueryFnData, TError, TData, TQueryKey>
  state: QueryState<TData, TError>

  private cache: QueryCache
  private observers: QueryObserver<any, any, any, any, any>[] // QueryObserver가 Query를 지켜보는 목록

  constructor(config: QueryConfig<TQueryFnData, TError, TData, TQueryKey>) {
    super()

    this.observers = []
    this.cache = config.cache
    this.queryKey = config.queryKey
    this.queryHash = config.queryHash
    this.state = this.initialState
  }

  dispatch() {
    // Query의 상태가 변할 때마다 변화를 전달하는 역할
    // 데이터가 업데이트되면 상태를 업데이트하고
    // 모든 observers에게 알려줘서 화면에 변화를 반영한다.
  }

  addObserver() {}
  removeObserver() {}
  fetch() {}
  // ...
}
```

`Query`는 `QueryCache`가 가진 `queries`, `queriesMap`에 저장되는 인스턴스이며 다음과 같은 정보를 가지고 있다.

- `Query` 인스턴스 자신을 소유하여 관리하고 있는 `QueryCache`에 대한 참조
- `useQuery`로 전달한 `queryFn`의 호출 결과 값 데이터
- `Query`가 갖고 있는 `QueryState` 데이터가 변경되는 경우 알림을 전달해야 하는 `QueryObserver` 목록

### **Query가 가진 QueryState의 업데이트가 발생하는 경우**

```jsx
export class Query extends Removable {
  constructor() {}

  dispatch(action: Action): void {
    const reducer = (state: QueryState): QueryState => {
      switch (action.type) {
        case 'fetch': return {...}
        case 'error': return {...}
        case 'success': return {
          ...state,
          data: action.data,
          dataUpdateCount: state.dataUpdateCount + 1,
          dataUpdatedAt: action.dataUpdatedAt ?? Date.now(),
          error: null,
          isInvalidated: false,
          status: 'success',
        }
      }
    }

    this.state = reducer(this.state)

    notifyManager.batch(() => {
      this.observers.forEach((observer) => {
        observer.onQueryUpdate(action)
      })
      this.cache.notify({ query: this, type: 'updated', action })
    })
  }
}
```

`QueryState`업데이트가 발생하는 경우 `QueryObserver`에게 지속해서 알림을 주기 위해 `Dispatch`, `Reducer`, `Action` 개념을 사용한다.

액션의 타입에 따라 리듀서를 통해 `QueryState`를 새롭게 업데이트한 후, 현재 `Query` 인스턴스를 구독하고 있는 `QueryObserver` 인스턴스의 메소드를 실행하는 것을 확인할 수 있다.

### `dispatch` 메서드

> `dispatch`는 `Query`에서 데이터를 가져오거나, 성공하거나, 실패했을 때 `QueryState`를 업데이트하는 역할을 한다. 각 `action.type`에 따라 `fetch`, `error`, `success` 상태가 `QueryState`에 저장된다.
>
> 1.  `fetch` 상태: 데이터가 로드 중일 때 상태를 설정.
> 2.  `error` 상태: 데이터를 가져오는 데 실패한 경우 상태를 업데이트.
> 3.  `success` 상태: 데이터를 성공적으로 가져왔을 때, 새로운 데이터와 함께 상태를 업데이트.

이때 `notifyManager.batch()` 안에 있는 부분이 중요한데, 이 부분은 **변화가 발생했을 때 모든 `QueryObserver`에게 알림을 보내서 컴포넌트가 새 데이터를 반영할 수 있게 한다**.

### QueryObserver

```jsx
export class QueryObserver extends Subscribable {

  constructor() {}

  onQueryUpdate(action) {
  const notifyOptions: NotifyOptions = {}

    if (action.type === 'success') {
      notifyOptions.onSuccess = !action.manual
    } else if (action.type === 'error' && !isCancelledError(action.error)) {
      notifyOptions.onError = true
    }

    this.updateResult(notifyOptions)if (this.hasListeners()) {
      this.updateTimers()
    }
  }
}
```

`QueryObserver*는 `Query`의 변화를 관찰하고 리스너에게 알리는 역할을 한다.

변화가 관찰되었을 때 내부의 최적화 과정을 거치게 되며 최종적으로 변화가 발생했음을 리스너에게 알릴 필요가 있는 경우 그 대상(현재의 문맥에서는 React 컴포넌트)에게 알려준다.

`Query` 클래스의 `dispatch` 메소드를 설명하던 이전 단락의 `observer.onQueryUpdate` 부분의 구현을 따라가 보면 `updateResult`메소드와 마주하게 된다.

- `onQueryUpdate`: `Query`가 업데이트되면 `QueryObserver`가 이 변화를 감지해 `onQueryUpdate`가 호출된다
- **변경 알림 설정**: 알림을 위해 `notifyOptions`가 설정되고, `updateResult`로 컴포넌트에 전달할 데이터를 준비한다.

```jsx
export class QueryObserver extends Subscribable {

  constructor() {}

  updateResult(notifyOptions?) {
    const prevResult = this.currentResult;
    const nextResult = this.createResult(this.currentQuery, this.options)

    // Only notify and update result if something has changed
    if (shallowEqualObjects(nextResult, prevResult)) {
      return
    }

    this.currentResult = nextResult

    // Determine which callbacks to trigger
    const defaultNotifyOptions: NotifyOptions = { cache: true }

    const shouldNotifyListeners = (): boolean => {
      // Checks if the listener needs to be notified that a change has occurred.
    }

    if (notifyOptions?.listeners !== false && shouldNotifyListeners()) {
      defaultNotifyOptions.listeners = true
    }

    this.notify({ ...defaultNotifyOptions, ...notifyOptions })
  }

  private notify(notifyOptions) {
    notifyManager.batch(() => {

      // Then trigger the listeners
      if (notifyOptions.listeners) {
        this.listeners.forEach(({ listener }) => {
          listener(this.currentResult)
        })
      }

    })
  }
}
```

`onQueryUpdate` 메소드에서 호출하는 `updateResult`의 함수를 보면 `Query` 상태의 전과 후를 비교하여 리스너에게 알림이 필요한지 확인한 후 `notify` 메소드를 실행하여 최신화된 `Query`의 상태를 전달한다.

`listeners`를 순회하며 함수를 실행하는 부분은 `QueryObserver`와 연결된 리액트 컴포넌트의 리렌더링을 지시하는 함수이다.

정리하면 현재까지 살펴본 React Query 내부 클래스 구조 및 클래스 간의 상호작용을 통해 하나의 흐름이 만들어진다.

![alt text](.//Images/react-query-2.png)

1. `Query` 상태에 변화가 발생한다.
2. `Query`와 연결된 `QueryObserver`에게 자신의 상태가 변화했음을 알린다.
3. `QueryObserver`는 자신과 연결된 `Listener`에게 관찰하고 있는 `Query`의 상태에 변화가 발생했는지를 판단하고 필요한 경우 `Listener`에게 알린다.
4. `Listener`는 업데이트된 `Query`의 상태를 참조하여 자신을 새로 갱신한다.

### **useQuery 훅의 실행 흐름과 React 컴포넌트의 View 업데이트**

다음은 리액트 컴포넌트 상에서 `useQuery` 훅을 호출하는 부분에서 시작하여

`Query`와 `QueryObserver`인스턴스의 생성 및 연결, `Query`의 상태 변경 시 `QueryObserver`를 통해 컴포넌트의 View가 업데이트되는 과정이다.

```jsx
export default function Todos() {
  const { data, error } = useQuery({
    queryKey: ["todos"],
    queryFn: fetchTodos,
  });
  return <div>{JSON.stringify(data)}</div>;
}
```

`useQuery`의 역할은 전달받은 인자를 정제하여 `useBaseQuery` 훅을 호출한다. 이때 정제된 옵션과 정의된 `QueryObserver` 클래스를 전달한다.

정리하자면

1. **useQuery 호출**: 컴포넌트에서 `useQuery`를 호출하면 React Query는 `QueryObserver`라는 **변화를 감지하는 감시자**를 만든다.
2. **옵션 처리**: `queryKey`와 `queryFn` 같은 옵션이 정리돼서, `useBaseQuery`라는 함수로 넘겨진다. 여기서 `queryKey`는 데이터의 ID 역할을 하고, `queryFn`은 데이터를 가져오는 함수이다.
3. **QueryObserver 생성**: `QueryObserver`는 **Query 상태를 지켜보고, 데이터가 변하면 알려주는 역할**을 한다.

```jsx
import { QueryObserver, parseQueryArgs } from "@tanstack/query-core";

export function useQuery(arg1, arg2?, arg3?) {
  const parsedOptions = parseQueryArgs(arg1, arg2, arg3);
  return useBaseQuery(parsedOptions, QueryObserver);
}
```

여기서 `useBaseQuery` 함수가 호출되면서, `QueryObserver`가 컴포넌트와 연결된다.

```jsx
export function useBaseQuery(options, Observer) {
  const queryClient = useQueryClient({ context: options.context });
  const defaultedOptions = queryClient.defaultQueryOptions(options);

  const [observer] = React.useState(
    () => new Observer(queryClient, defaultedOptions)
  );

  const result = observer.getOptimisticResult(defaultedOptions);

  useSyncExternalStore(
    React.useCallback(
      (onStoreChange) => {
        const unsubscribe = observer.subscribe(
          notifyManager.batchCalls(onStoreChange)
        );
        // Update result to make sure we did not miss any query updates
        // between creating the observer and subscribing to it.
        observer.updateResult();
        return unsubscribe;
      },
      [observer]
    ),
    () => observer.getCurrentResult(),
    () => observer.getCurrentResult()
  );

  // Handle result property usage tracking
  return !defaultedOptions.notifyOnChangeProps
    ? observer.trackResult(result)
    : result;
}
```

`useBaseQuery` 훅의 구현을 살펴보면 하나의 `QueryObserver`가 인스턴스화 되기 시작하는 것을 확인할 수 있다.

`useQuery` 훅을 호출하는 각각의 리액트 컴포넌트는 하나의 `QueryObserver` 인스턴스와 연결관계를 맺게 된다.

`Query`의 변화를 관찰한 `QueryObserver`는 리액트 컴포넌트를 리렌더링 시키기 위해 `useSyncExternalStore`훅을 이용한다.

이 훅은 첫 번째 인자로 함수를 전달하게 되는데 이 함수의 콜백 파라미터는 이 훅을 호출하는 리액트 컴포넌트에 대해 리렌더링을 지시하는 함수(이 문맥에서는 **onStoreChange**)이다.

이 리렌더링 지시 함수가 이동하는 곳을 보면 아래와 같다.

```jsx
export class Subscribable<TListener extends Function = Listener> {
  protected listeners: Set<{ listener: TListener }>

  constructor() {
    this.listeners = new Set()
    this.subscribe = this.subscribe.bind(this)
  }

  subscribe(listener: TListener): () => void {
    const identity = { listener }
    this.listeners.add(identity)

    this.onSubscribe()

    return () => {
      this.listeners.delete(identity)
      this.onUnsubscribe()
    }
  }
}
```

이 리액트 컴포넌트의 리렌더링을 지시하는 함수 `onStoreChange`는 `observer.subscribe(onStoreChange)` 문장을 따라 이동하는데 `subscribe` 메소드는 `QueryObserver`클래스의 부모 클래스인 `Subscribable`에서 구현하고 있다.

`Set` 자료형인 `listeners` 필드에 함수가 추가되는 것을 확인할 수 있다.

```jsx
export class QueryObserver extends Subscribable {

  constructor() {}

  private notify(notifyOptions) {
    notifyManager.batch(() => {

      // Then trigger the listeners
      if (notifyOptions.listeners) {
        this.listeners.forEach(({ listener }) => {
          listener(this.currentResult)
        })
      }

    })
  }
}
```

다시 `QueryObserver` 클래스로 돌아와 `notify` 메소드에 위치한 로직에서 `listeners`에 대해 순회하여 `listener`함수를 실행하는 부분은 현재의 `QueryObserver` 인스턴스와 연결된 리액트 컴포넌트들에 대해 리렌더링을 지시하는 부분이다.

결과적으로 컴포넌트가 리렌더링 되면서 변경된 `Query`의 데이터를 표시하게 된다.

정리하면 리액트 컴포넌트상에서의 `useQuery` 훅을 호출하게 되면 `Query`의 변화를 관찰하는 `QueryObserver`가 생성되어 컴포넌트와 연결을 맺게 되고 `Query`의 변화에 따라 컴포넌트는 리렌더링이 발생하게 된다.

![alt text](.//Images/react-query-3.png)

1. 리액트 컴포넌트에서 `useQuery` 훅을 호출한다.
2. `useBaseQuery` 훅으로 정제된 옵션과 `QueryObserver` 클래스가 전달된다.
3. `useBaseQuery` 훅의 실행과정에서 리액트 컴포넌트는 `QueryObserver` 인스턴스와 연결된다.
4. `QueryObserver`가 `Query`의 변화를 관찰하여 `notify` 메소드를 실행하게 되면 리액트 컴포넌트의 리렌더링이 발생하게 된다.

## React Query의 주요 특징 및 사용법

### 1. **캐싱 (Caching)**

React Query의 가장 큰 장점 중 하나는 데이터를 **캐싱**하여 동일한 데이터에 대한 반복적인 API 호출을 줄여주는 것이다. 이를 통해 서버 부하를 감소시켜 성능 최적화에 기여한다.

- **최신 데이터 판별**: React Query는 서버 데이터를 불러와 캐싱한 후에도 실제 서버 데이터와의 비교를 통해 최신 여부를 판별한다. 이를 위해 데이터를 **fresh (최신)** 또는 **stale (구버전)** 상태로 구분하여 관리하게된다.

### 2. **데이터 갱신 시점**

React Query는 적절한 시점에 데이터를 갱신하여 항상 최신 정보를 제공하는데, 다음과 같은 경우 데이터를 다시 가져온다.

1. 화면에 포커스가 들어올 때 (`refetchOnWindowFocus`)
2. 새로운 컴포넌트가 마운트될 때 (`refetchOnMount`)
3. 네트워크가 재연결될 때 (`refetchOnReconnect`)

이를 위해 React Query는 몇 가지 기본 옵션을 제공한다.

- `refetchOnWindowFocus` (기본값: true)
- `refetchOnMount` (기본값: true)
- `refetchOnReconnect` (기본값: true)
- `staleTime` (기본값: 0)
- `cacheTime` (기본값: 5분)

이 옵션들을 통해 필요한 경우에만 데이터가 갱신되도록 설정할 수 있다.

### 3. **staleTime과 cacheTime**

- **staleTime**: 데이터가 fresh 상태에서 stale 상태로 변경되는 데 걸리는 시간이다. 설정한 staleTime 동안에는 refetch 트리거가 발생해도 데이터 갱신이 일어나지 않는다.
- **cacheTime**: 데이터가 inactive 상태로 남아 있는 시간이다. 컴포넌트가 unmount되면 데이터를 캐시에 유지하고, cacheTime이 지나면 데이터는 메모리에서 삭제된다. cacheTime이 지나지 않은 경우 캐싱된 데이터가 임시로 표시된다.

### 4. **데이터 관리 구조**

React Query는 React의 Context API 기반으로 동작하며, 전체 데이터를 관리하는 `QueryClient`가 존재한다.

`QueryClient`는 Query의 key를 기준으로 데이터를 저장하여, 데이터를 관리하는 Context Store와 같은 역할을 한다. 이를 통해 필요한 경우 `QueryClient`를 단순한 데이터 저장소처럼 활용할 수 있다.

React Query의 이러한 특성들은 항상 최신의 데이터를 제공하여 사용자 경험을 개선하고 성능을 최적화하는 데 기여한다.

## 대표적인 기능 및 사용법

React-Query에서 data fetching을 위해 제공하는 대표적인 기능들로는 기본적으로 GET 에는 useQuery가, PUT, UPDATE, DELETE에는 useMutation이 사용된다.

### useQuery

- 첫 번째 파라미터로 unique key를 포함한 배열이 들어간다. 이후 동일한 쿼리를 불러올 때 유용하게 사용된다.
- 첫 번째 파라미터에 들어가는 배열의 첫 요소는 unique key로 사용되고, 두 번째 요소부터는 query 함수 내부의 파라미터로 값들이 전달된다.
- 두 번째 파라미터로 실제 호출하고자 하는 비동기 함수가 들어간다. 이때 함수는 Promise를 반환하는 형태여야 한다.
- 최종 반환 값은 API의 성공, 실패 여부, 반환값을 포함한 객체이다.

### Example

```jsx
import {
  QueryClient,
  QueryClientProvider,
  useQuery,
} from "@tanstack/react-query";

const queryClient = new QueryClient();

export default function App() {
  return (
    <QueryClientProvider client={queryClient}>
      <Example />
    </QueryClientProvider>
  );
}

function Example() {
  const { isPending, error, data } = useQuery({
    queryKey: ["repoData"],
    queryFn: () =>
      fetch("https://api.github.com/repos/tannerlinsley/react-query").then(
        (res) => res.json()
      ),
  });

  if (isPending) return "Loading...";

  if (error) return "An error has occurred: " + error.message;

  return (
    <div>
      <h1>{data.name}</h1>
      <p>{data.description}</p>
      <strong>👀 {data.subscribers_count}</strong>{" "}
      <strong>✨ {data.stargazers_count}</strong>{" "}
      <strong>🍴 {data.forks_count}</strong>
    </div>
  );
}
```

useQuery 함수가 반환하는 객체를 보면 isPending 을 통해 로딩 여부를, error 를 통해 에러 발생 여부를, data를 통해 성공 시 데이터를 반환할 수 있다.

isPending과 error를 이용하여 각 상황 별 분기를 쉽게 진행할 수 있다.

### useQuery 동기적으로 실행

- useQuery에서 `enabled` 옵션을 사용하면 비동기 함수인 useQuery를 동기적으로 사용 가능하다.
- useQuery의 세 번째 인자로 다양한 옵션 값들이 들어가는데, 여기서 `enabled`에 값을 대입하면 해당 값이 true일 때 useQuery를 동기적으로 실행한다!

```jsx
const {
  data: todoList,
  error,
  isFetching,
} = useQuery({
  queryKey: ["todos"],
  queryFn: fetchTodoList,
});

const {
  data: nextTodo,
  error,
  isFetching,
} = useQuery({
  queryKey: ["nextTodos"],
  queryFn: fetchNextTodoList,
  enabled: !!todoList, // true가 되면 fetchNextTodoList를 실행한다
});
```

### useQueries

여러 개의 useQuery를 한 번에 실행하고자 하는

경우, 기존의 `Promise.all()`처럼 묶어서 실행할 수 있도록 도와준다!

```jsx
const ids = [1, 2, 3];
const results = useQueries({
  queries: ids.map((id) => ({
    queryKey: ["post", id],
    queryFn: () => fetchPost(id),
    staleTime: Infinity,
  })),
});
```

두 query에 대한 반환값이 배열로 묶여 반환된다.

만일 반환된 배열에 대해 통합된 값을 불러오고 싶다면, 아래와 같이 combine 설정을 통해 데이터를 한 번에 반환할 수 있다.
<br>이외에도 배열을 다루는 메서드들을 이용해 반환값에 대한 전처리를 수행할 수 있다.

```jsx
const ids = [1, 2, 3];
const combinedQueries = useQueries({
  queries: ids.map((id) => ({
    queryKey: ["post", id],
    queryFn: () => fetchPost(id),
  })),
  combine: (results) => {
    return {
      data: results.map((result) => result.data),
      pending: results.some((result) => result.isPending),
    };
  },
});
```

### useMutation

- 위에서 언급한 것처럼 PUT, UPDATE, DELETE 와 같이 값을 변경할 때 사용하는 API다. 반환값은 useQuery와 동일하다.

### 예시

```jsx
function App() {
  const mutation = useMutation({
    mutationFn: (newTodo) => {
      return axios.post("/todos", newTodo);
    },
  });

  return (
    <div>
      {mutation.isLoading ? (
        "Adding todo..."
      ) : (
        <>
          {mutation.isError ? (
            <div>An error occurred: {mutation.error.message}</div>
          ) : null}

          {mutation.isSuccess ? <div>Todo added!</div> : null}

          <button
            onClick={() => {
              mutation.mutate({ id: new Date(), title: "Do Laundry" });
            }}
          >
            Create Todo
          </button>
        </>
      )}
    </div>
  );
}
```

- 위의 코드에서 볼 수 있듯이 반환값은 useQuery와 동일하지만, 처음 사용 시에 post 비동기 함수를 넣어주었다. 이때 useMutation의 첫 번째 파라미터에 비동기 함수가 들어가고, 두 번째 인자로 상황 별 분기 설정이 들어간다는 점이 차이이다.
- 실제 사용 시에는 `mutation.mutate` 메서드를 사용하고, 첫 번째 인자로 API 호출 시에 전달해주어야하는 데이터를 넣어주면 된다

[참고]

- [React Query의 구조와 useQuery 실행 흐름 살펴보기](https://fe-developers.kakaoent.com/2023/230720-react-query/)
- [[React-Query] React-Query 개념잡기](https://velog.io/@kandy1002/React-Query-%ED%91%B9-%EC%B0%8D%EC%96%B4%EB%A8%B9%EA%B8%B0)
