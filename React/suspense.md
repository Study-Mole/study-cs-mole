# React Suspense

React Suspense는 컴포넌트의 렌더링을 일시 중지하고 데이터 로딩을 기다릴 수 있게 해주는 React의 기능이다.

대기하는 동안 다른 컴포넌트를 렌더링할 수 있기 때문에 기존보다 자연스러운 사용자 경험을 유도할 수 있다.

또한, Code Splitting과 함께 사용하면 초기 로딩 속도를 개선하는데에도 도움이 된다.

## Suspense 사용하기

아래 간단한 사용법을 보자.

```js
<Suspense fallback={<Loading />}>
  <SomeComponent />
</Suspense>
```

`fallback`에는 실제 UI의 로딩이 끝날 때까지 대신 보여줄 컴포넌트를 넣어준다. 보통 스피너나 스켈레톤을 사용한다.

만약 Suspense가 컨텐츠를 보여준 다음 다시 suspend 상태에 들어가면 `fallback`을 노출한다.

### Fetch-on-Render

Suspense가 없이 로딩을 처리하던 기존 방식을 `Fetch-on-Render`라고 한다. 아래의 코드를 살펴보자.

```js
function App() {
  const [data, setData] = useState(null);

  useEffect(() => {
    fetchData().then((result) => setData(result));
  }, []);

  if (!data) return <div>로딩 중...</div>;
  return <div>{data}</div>;
}
```

data가 없을 때에는 `로딩 중...` 컴포넌트를 반환한다. useEffect에서의 `fetchData` 요청이 완료되면 `setData`가 실행되면서 재 렌더링을 하고 원래 목적인 컴포넌트를 반환한다.

이렇게 렌더링 중에 데이터를 요청하는 방식은 컴포넌트가 중첩될 수록 문제가 발생한다. 자식 컴포넌트는 부모 컴포넌트의 데이터 요청과 렌더링이 완료된 이후에야 렌더링을 시도한다. 중첩될 수록 로딩 시간은 점점 지연되며, 하위 요소로 전파되는 로딩은 좋을 수가 없다.

### Render-as-You-Fetch

Suspense를 사용하면 데이터 로딩 완료 후에 리렌더링을 하는 것이 아니라, 로딩과 함께 본 렌더링을 시작할 수 있다.

```js
const resource = fetchData();

function App() {
  return (
    <Suspense fallback={<div>로딩 중...</div>}>
      <DataComponent />
    </Suspense>
  );
}

function DataComponent() {
  const data = resource.user.read();
  return <div>{data}</div>;
}
```

위 코드의 `resource`는 Suspense를 적용할 수 있도록 만들어진 데이터 패칭 구현을 개념적으로 나타낸 것으로 `react-query`같은 라이브러리를 생각하면 된다.

Suspense를 사용하면 데이터가 없거나 로딩 중일 때는 해당 컴포넌트의 렌더링을 건너뛰고 다른 컴포넌트를 먼저 렌더링한다. 그리고 가장 가까운 부모 Suspense의 fallback으로 지정된 컴포넌트를 화면에 대신 보여준다. fallback은 보이는 것을 대신할 뿐 백그라운드에서 Suspense된 컴포넌트의 렌더링은 시도된다.

Suspense는 그렇다고 아무 상황에서나 적용할 수 있는 것은 아니고, 패칭 역할로 `resource`를 사용한 것처럼 데이터 요청이 **Suspense에 맞는 방식**으로 구현되어 있어야 한다. 그 방식은 **Promise를 throw**하는 것이다.

## Suspense 사용예제

앞의 설명에서 Suspense에 맞는 방식에는 `React.lazy`와 `Tanstack Query`가 있다. 이들을 사용하여 Suspense를 적용하는 방법을 살펴보자.

### `React.lazy`

`React.lazy`를 사용하면 컴포넌트가 필요할 때 코드를 가져오고 실행하게 된다. 컴포넌트 코드를 가져오고 실행하는 딜레이가 발생하므로 Suspense를 사용하면 더 자연스러운 UI를 제공할 수 있다.

```js
const lazyComponent = React.lazy(() => import("./LazyComponent"));

const App = () => {
  return (
    <Suspense fallback={<Loading />}>
      <LazyComponent />
    </Suspense>
  );
};
```

위 코드와 같이 React.lazy 함수를 사용하여 가져온 컴포넌트 상위에 Suspense를 사용하면 간단하게 컴포넌트 코드를 가져오고 지연 시간에 대한 로딩 처리가 가능하다.

### Tanstack Query

React Query는 비동기 작업을 돕는 라이브러리로 API를 호출하여 서버에서 데이터를 가져오는 작업을 할 때 사용된다. React Query의 `useSuspenseQuery` 훅을 사용하면 Suspense를 쉽게 적용할 수 있다.

```js
import { useSuspenseQuery } from "@tanstack/react-query";

const SomeComponent = () => {
  const { data } = useSuspenseQuery({
    queryKey: ["posts"],
    queryFn: async () => {
      const result = await fetch("fetch url");
      return result.json();
    },
  });
  return <div>{data}</div>;
};

export default SomeComponent;
```

## React Suspense를 사용하는 이유

- 사용자 경험 개선
  - 데이터 로딩으로 인한 딜레이 동안에도 자연스러운 사용자 경험을 제공할 수 있다.
  - 스켈레톤 UI 등을 활용하면 실제 콘텐츠가 로드되기 전에 미리 레이아웃을 볼 수 있다.
- 초기 로딩 속도 개선
  - `lazy`를 사용하여 초기에 필요한 컴포넌트만 로드하고, 나머지는 요구되었을 때 로드할 수 있다.
- 에러 처리 용이
  - Suspense를 사용하면 로딩 중 발생한 에러를 쉽게 처리할 수 있다.
  - `Error Boundary`와 함께 사용하면 에러가 발생했을 때 사용자에게 적절한 피드백을 제공하고, 애플리케이션의 다른 부분은 정상적으로 동작하도록 할 수 있다.

<br/>

[참고]

- [[React] Suspense의 동작 원리](https://velog.io/@shinhw371/React-suspense-throw)
- [[React] Suspense](https://beomy.github.io/tech/react/suspense/)
