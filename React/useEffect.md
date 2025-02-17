# **useEffect, 오남용을 피하는 5가지**

리액트의 `useEffect` 훅은 컴포넌트 생애주기에 맞춰 비동기 로직, 외부 리소스 연결, 상태 업데이트 등을 손쉽게 처리할 수 있다는 점에서 매우 유용하다. 그러나 무분별하게 사용하면 불필요한 렌더링, 복잡한 데이터 흐름, 관리가 어려운 코드 등 다양한 문제를 야기할 수 있다. 아래 다섯 가지 예시를 통해, `useEffect`를 보다 신중하고 효율적으로 쓰는 방법을 알아보자.

## 1. 경쟁 상태(race condition)와 클린업 처리

### 1-1. 경쟁 상태가 발생할 수 있는 코드 예시

다음 코드는 `counter` 값이 변경될 때마다 비동기 요청을 보내, 그 결과를 `response` 상태에 저장한다. 문제는 요청 순서와 응답 도착 순서가 어긋날 수 있다는 점이다. 이를테면 **이전 요청이 늦게 응답을 반환해** 최신 상태를 덮어쓰는 상황이 발생할 수 있다.

```jsx
function RaceConditionExample() {
  const [counter, setCounter] = useState(0);
  const [response, setResponse] = useState(0);
  const [isLoading, setIsLoading] = useState(false);

  useEffect(() => {
    const request = async (requestId) => {
      setIsLoading(true);
      await sleep(Math.random() * 3000); // 0~3000ms 동안 임의로 지연
      setResponse(requestId); // 이전 요청이 나중에 도착하면 최신 응답을 덮어쓸 수 있다
      setIsLoading(false);
    };
    request(counter);
  }, [counter]);

  const handleClick = () => {
    setCounter((prev) => prev + 1);
  };

  return (
    <>
      <div>response: {response}</div>
      <button onClick={handleClick}>Increment</button>
    </>
  );
}
```

위 코드처럼 요청 1이 3초 뒤 도착하고, 요청 2가 1초 뒤 도착하는 식으로 순서가 뒤섞이면, `response`가 잘못된 값으로 갱신될 수 있다.

### 1-2. 클린업 함수를 활용한 해결책

다음 예시처럼 이펙트 내부에 `ignore` 같은 플래그를 두어, 이펙트가 클린업될 때 이를 설정해두면 **이전 이펙트에서 진행 중이던 응답**을 무시할 수 있다.

```jsx
useEffect(() => {
  let ignore = false;

  const request = async (requestId) => {
    setIsLoading(true);
    await sleep(Math.random() * 3000);
    if (!ignore) {
      setResponse(requestId);
      setIsLoading(false);
    }
  };

  request(counter);

  return () => {
    ignore = true; // 언마운트되거나 deps가 변경되기 직전, 이전 이펙트를 무시
  };
}, [counter]);
```

이와 같이 처리하면 가장 마지막에 시작된 이펙트가 아니면 결과를 반영할 수 없게 되어, 경쟁 상태를 예방할 수 있다. 또한 AbortController 같은 API로 요청 자체를 취소해도 유사한 효과를 얻을 수 있다.

## 2. 불필요한 렌더링을 일으키는 잘못된 데이터 흐름

### 2-1. 이벤트 핸들러에서 이미 상태를 변경했는데, 추가로 `useEffect`에서 또 변경하는 경우

다음 코드는 “버튼 클릭 → 자식 상태 변경 → 다시 부모 상태를 변경”을 **두 단계 렌더링**으로 처리한다. 첫 번째 렌더링은 자식이 로컬 상태를 갱신하는 순간 일어나고, 두 번째 렌더링은 `useEffect`가 부모 상태를 갱신하면서 일어난다.

```jsx
function Parent() {
  const [someState, setSomeState] = useState();
  return <Child onChange={(v) => setSomeState(v)} />;
}

function Child({ onChange }) {
  const [isOn, setIsOn] = useState(false);

  useEffect(() => {
    onChange(isOn); // 부모 컴포넌트를 다시 렌더링하게 만든다
  }, [isOn, onChange]);

  function handleClick() {
    setIsOn(!isOn); // 로컬 상태 변경으로 이미 렌더링이 발생한다
  }

  return <button onClick={handleClick}>Toggle</button>;
}
```

위 로직에서는 **버튼을 누르는 한 번의 이벤트**에 대해 **두 번 렌더링**이 발생한다. 사실상 버튼 클릭 시점에 로컬 상태와 부모 상태를 함께 갱신하는 것이 충분하다.

```jsx
function Parent() {
  const [someState, setSomeState] = useState();
  return <Child onChange={setSomeState} />;
}

function Child({ onChange }) {
  const [isOn, setIsOn] = useState(false);

  function handleClick() {
    const newValue = !isOn;
    setIsOn(newValue);
    onChange(newValue);
  }

  return <button onClick={handleClick}>Toggle</button>;
}
```

이렇게 수정하면 이벤트가 발생하는 시점에 한 번에 상태를 갱신하므로, **추가적인 `useEffect`**가 필요하지 않고 렌더링 횟수도 줄어든다.

### 2-2. “자식에서 fetch/mutate한 결과를 부모에 전달”하는 복잡한 역방향 흐름

```jsx
function Parent() {
  const [data, setData] = useState(null);
  return <Child onFetched={setData} />;
}

function Child({ onFetched }) {
  const data = useFetchData();

  useEffect(() => {
    if (data) {
      onFetched(data);
    }
  }, [data, onFetched]);

  return <div>{JSON.stringify(data)}</div>;
}
```

위 구조는 자식이 데이터를 가져와 부모로 전달하는 역방향 데이터 흐름을 형성한다. 프로젝트가 커질수록 “어느 시점에 부모가 업데이트되는지” 추적하기가 어려워진다.  
일반적으로는 **부모가 직접 fetch**한 뒤 결과를 자식으로 **props** 형태로 넘기는 방식이 명료하다. 자식이 네트워크 요청을 수행해야 한다면, **성공/실패 시점에 콜백**을 호출하도록 설계하면 된다.

## 3. 앱 런타임 중 **단 한 번**만 실행해야 하는 초기화 로직

예컨대 **인증**, **SDK 초기화** 등 전체 애플리케이션에서 오직 한 번만 호출되어야 하는 로직을 `useEffect(() => {}, [])` 안에 넣는 경우가 많다.

```jsx
function App() {
  useEffect(() => {
    someOneTimeLogic(); // 단 한 번만 실행하고 싶다
  }, []);
  // ...
}
```

### 3-1. 왜 문제가 될까?

- **Strict Mode**에서는 컴포넌트를 마운트-언마운트했다가 다시 마운트하는 과정을 의도적으로 반복할 수 있다.
- **동시성 모드**나 향후 리액트의 다양한 최적화 환경에서도, “한 번만 마운트된다”는 전제를 항상 보장하기 어렵다.

결과적으로 “정말 한 번만” 실행하고자 해도 **개발 환경이나 특정 모드**에서는 여러 번 실행될 수 있으므로, 의도와 다른 결과가 생길 가능성이 있다.

### 3-2. 대안

- **전역 변수 혹은 플래그**를 사용해 “이미 실행했는지”를 체크한다.

  ```jsx
  let didInit = false;

  function App() {
    useEffect(() => {
      if (!didInit) {
        didInit = true;
        someOneTimeLogic();
      }
    }, []);
    // ...
  }
  ```

- **컴포넌트가 아닌 모듈 스코프**에서 직접 한 번 실행한다.

  ```jsx
  // App.js 상단
  if (typeof window !== "undefined") {
    someOneTimeLogic(); // 애플리케이션 시작 시점에 실행
  }

  function App() {
    // ...
  }
  ```

위와 같은 방식으로 “이미 실행했는지”를 추적하거나 컴포넌트 바깥에서 실행하면, 의도치 않게 여러 번 마운트되는 경우에도 안전하다.

## 4. 언마운트된 컴포넌트에서 setState를 호출해도 문제가 없다

과거 리액트 버전(17 이하)에서는 “언마운트된 컴포넌트에서 `setState`를 호출하면 경고가 발생한다”는 제약이 있었다. 이를 피하기 위해 `isMounted` 같은 훅을 만들고, 컴포넌트가 언마운트된 상태에서는 `setState`를 무시하는 코드를 작성하곤 했다.

```jsx
function MyComponent() {
  const [loading, setLoading] = useState(false);
  const mountedRef = useRef(true);

  useEffect(() => {
    return () => {
      mountedRef.current = false;
    };
  }, []);

  async function handleDeleteBill(id) {
    setLoading(true);
    await axios.delete(`/bill/${id}`);
    if (mountedRef.current) {
      setLoading(false);
    }
  }

  return (
    <button onClick={() => handleDeleteBill(...)} disabled={loading}>
      Delete Bill
    </button>
  );
}
```

하지만 **React 18**부터는 이 경고가 제거되었고, 공식 문서에서도 “언마운트된 컴포넌트에서 `setState`를 호출해도 문제가 없다”고 안내한다.

1. 언마운트된 컴포넌트에서 `setState`를 호출해도 **메모리 누수**가 일어나지 않는다.
2. 향후 리액트가 **상태 보존 메커니즘** 등을 강화함에 따라, 탭 전환이나 컴포넌트 재마운트 시 기존 상태를 재활용할 수 있게 될 수 있다. 이때 억지로 `setState`를 막으면 올바른 상태 보존에 해가 될 수 있다.
3. `isMounted`를 추적하는 로직 자체가 `useEffect` 등의 추가 코드와 복잡도를 늘린다.

다음은 별도의 `isMounted` 추적 없이 상태를 갱신하는 예시이다.

```jsx
function MyComponent() {
  const [loading, setLoading] = useState(false);

  async function handleDeleteBill(id) {
    setLoading(true);
    await axios.delete(`/bill/${id}`);
    // 언마운트된 상태라도 호출이 가능하다
    setLoading(false);
  }

  return (
    <button onClick={() => handleDeleteBill(...)} disabled={loading}>
      Delete Bill
    </button>
  );
}
```

## 5. 정리: useEffect를 남발하지 않으려면

### 5-1. 핵심 요점

1. **데이터 흐름은 “부모 → 자식”이 기본**

   - 자식에서 직접 부모로 데이터를 전달하는 “역방향” 구조는 프로젝트가 커질수록 유지 보수를 어렵게 만든다.
   - 부모에서 데이터를 fetch/mutate하고, 자식에게 **props**로 전달하거나, 자식이 필요한 시점에 **콜백(onSuccess/onError)**을 호출하는 방식이 권장된다.

2. **비동기 로직은 경쟁 상태를 주의**

   - 여러 요청이 동시에 발생하는 상황에서 **늦게 도착한 이전 응답**이 최신 상태를 덮어쓰지 않도록, 클린업 함수나 AbortController 등을 이용해 요청을 무시하거나 취소한다.

3. **굳이 `useEffect` 내부에서 로컬 상태를 다시 설정하지 않는다**

   - 이벤트 핸들러(예: onClick) 안에서 이미 처리할 수 있는 경우에는 즉시 `setState`를 호출하는 것이 불필요한 렌더링을 줄인다.

4. **“앱 전체에서 딱 한 번” 실행해야 하는 로직은 `useEffect`에만 의존하기 어렵다**

   - Strict Mode 등에서 여러 번 마운트될 수 있으므로, 전역 변수 혹은 모듈 스코프에서 ‘실행 여부’를 체크하거나 한 번만 실행하는 방법을 마련한다.

5. **언마운트된 컴포넌트의 `setState`를 억지로 막을 필요가 없다**
   - React 18 이후로 경고가 사라졌고, 메모리 누수 문제도 발생하지 않는다.
   - 향후 상태 보존 기능에 대해서도 유연하게 대응하기 위해, 불필요한 `isMounted` 로직을 추가하지 않는 것이 유리하다.

### 5-2. 그렇다면 `useEffect`는 언제 쓰면 되는가?

- **외부 리소스 초기화 및 구독(Subscription)**  
  예: WebSocket, Firebase, 분석 도구, 맵(Map) API 등을 연동하고, 언마운트 시점에 구독 해제가 필요한 경우.
- **(컴포넌트 라이프사이클별로) 특정 로직을 수행**  
  전역이 아니라 특정 컴포넌트가 마운트될 때마다 실행해야 하는 경우.
- **구독 해제 등이 필요한 클린업 로직**  
  이벤트 리스너 제거, 소켓 연결 해제 등의 동작이 필수적인 경우.
- **퍼포먼스 최적화**  
  방대한 데이터 처리나 필터링을 효율적으로 진행하려고 `useMemo`나 `useEffect`를 결합해 메모이제이션 로직을 구성하는 경우.

[참고]

- [Leave useEffect Alone!](https://medium.com/meliopayments/leave-useeffect-alone-d522a60bbbd4)
