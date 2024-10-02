# React

- [LifeCycle](#lifecycle)
    - [함수형 컴포넌트의 LifeCycle](#함수형-컴포넌트의-lifecycle)
- []()

<br/>

## LifeCycle
React 컴포넌트에는 생명 주기가 존재한다. 컴포넌트의 수명은 렌더링되기 전 준비 과정에서 시작하여 페이지에서 사라질 때 끝이난다.

생성자를 통해서 필요한 메모리를 할당하고, 객체의 역할이 끝나면 소명자를 통하여 메모리를 반환한다. <br/> 컴퓨터의 자원은 한정적이기 때문이다. 생명 주기의 가장 큰 존재 이유는 메모리 비우기에 있다.

생명 주기는 크게 세 가지 유형으로 나뉜다.<br/>
**각 컴포넌트들을 생성(Mount)하고, 업데이트(Update)하고, 제거(Unmount)하는 단계를 차례로 겪는다.**

때로는 생명 주기에서 각 단계별로 필요한 순간에 요구되는 작업들을 끼워넣을 수 있는 메서드들이 존재한다.

이러한 생명 주기 메서드들은 클래스형 컴포넌트에서만 사용할 수 있으며, 함수형 컴포넌트에서는 사용할 수 없다.<br/> 함수형 컴포넌트에서는 Hooks로 유사한 동작을 만들어 낼 수 있다.


### 1. Mount
DOM이 생성되고, 웹 브라우저 상에서 나타나는 것을 뜻한다.

**`constructor`**

클래스 인스턴스 객체를 생성하고 초기화하는 메서드로 리액트에서 컴포넌트가 생성될 때 가장 처음으로 실행된다.
```JSX
class MyComponent extends React.Component {
    constructor(props) {
        super(props);
        this.state = { count: 0 };
    }
}
```
클래스형 컴포넌트에서는 초기 state를 정할 때 `constructor`를 사용해야 한다.<br/>
메서드 바인딩, state 초기화 작업이 필요없다면, 해당 React 컴포넌트에는 constructor을 구현하지 않아도 된다.

#### `static getDerivedStateFromProps`

props로 받아온 값을 state에 넣어주는 용도로 사용한다. Mount, Update시에 호출된다.
```JSX
class MyComponent extends React.Component {
    static getDerivedStateFromProps(nextProps, prevState) {
        if(nextProps.value !== prevState.value) {
            return { value: nextProps.value }
        }
        return null
    }
}
```
`static` 메서드이기 때문에 this를 호출할 수 없다. <br/>
`render` 메서드를 호출하기 직전에 호출되며 렌더링 때마다 매번 실행된다. <br/>
state를 갱신하기 위한 객체를 반환하거나, null을 반환하여 아무 것도 갱신하지 않을 수 있다.

#### `render`

컴포넌트를 렌더링 해주는 메서드이다.
클래스 컴포넌트에서 반드시 구현되어야 하는 유일한 필수 메서드이다.
```jsx
render() { ... }
```


**`componentDidMount`**

컴포넌트의 첫 번째 렌더링이 끝나면 호출되는 메서드이다.<br/>
이 메서드가 호출되는 시점에는 화면에 컴포넌트가 나타난 상태인 것이다.
```jsx
componentDidMount() { ... }
```

### 2. Update
React 컴포넌트에 변화가 있을 때를 업데이트라고 한다.

1. 부모 컴포넌트가 넘겨주는 props가 바뀔 때
2. setState 함수를 통해 state가 변경될 때
3. 부모 컴포넌트가 리렌더링 될 때
4. this.forceUpdate로 강제 렌더링을 트리거 할 때

컴포넌트가 업데이트 되는 시점에 어떤 메서드들이 호출되는지 알아보자.

<br/>

**`getDerivedStateFromProps`**

[위](#static-getderivedstatefromprops)에서 설명했듯이 props 또는 state가 변경되었을 때 render 메서드 이전에 실행된다.

**`shouldComponentUpdate`**

컴포넌트를 리렌더링 할지 말지 결정하는 메서드이다.
props 또는 state가 변경되어 렌더링이 발생하기 직전에 호출된다.
```jsx
shouldComponentUpdate(nextProps, nextState) { 
    // 두더지들이 4명 미만일 경우 리렌더링 하지 않습니다.
    return nextState.mole < 4;
}
```
기본값은 true이며 false를 리턴하면 렌더링은 취소된다. (주로 성능 최적화에 사용됨)

**`render`**

[Mount의 render](#render)와 동일하다.

#### `getSnapShotBeforeUpdate`

컴포넌트 변화를 DOM에 반영하기 바로 직전에 호출되는 메서드이다.
```jsx
getSnapShotBeforeUpdate(prevProps, prevState) {
    if(prevProps.mole !== this.state.mole) {
        return this.myRef.style.color
    }
    return null;
}
```
DOM 상태가 변경되기 전의 값을 반환하여 사용이 가능하다. <br/>
다음에 발생하게 될 componentDidUpdate 메서드에서 그 반환값을 사용할 수 있다.

**`componentDidUpdate`**

리렌더링 작업이 완료된 직후 실행되는 메서드이다.
최초 렌더링 시에는 호출되지 않는다.
```jsx
componentDidUpdate(prevProps, prevState, snapshot) { 
    if(snapshot) console.log('업데이트 되기 전 색상: ', snapshot);
}
```
3번째 파라미터로 [getSnapshotBeforeUpdate](#getsnapshotbeforeupdate)에서 반환한 값을 조회할 수 있다.

### 3. Unmount
Unmount는 컴포넌트가 화면에서 제거되는 것을 의미한다. 관련된 메서드는 하나이다.

#### `componentWillUnmount`

컴포넌트가 사라지기 직전에 호출되는 메서드이다.<br/>
주로 DOM에 등록했었던 이벤트들을 제거해주는 용도로 사용된다.
```jsx
componentWillUnmount()
```

<br/>
위의 내용을 그림으로 이미지로 정리해보자.

![컴포넌트 생명 주기](./images/life-cycle.png)

## 함수형 컴포넌트의 LifeCycle
React의 함수형 컴포넌트는 클래스형 컴포넌트를 사용할 필요가 없을 정도로 강력해졌으며, 이를 가능하게 하는 것은 바로 Hooks이다. Hooks는 함수형 컴포넌트에서 클래스형 컴포넌트의 라이프사이클 메서드를 대체할 수 있도록 돕는다.

`useEffect`를 사용하여 많은 메서드를 대체할 수 있다.
`useEffect`는 기본적으로 컴포넌트가 렌더링된 후 실행되며, 의존성 배열을 사용해서 실행 시점을 제어할 수 있다.

#### `componentDidMount`
컴포넌트의 첫 번째 렌더링이 끝나면 호출되는 메서드이다.<br/>
useEffect에 빈 의존성 배열을 사용하여 구현할 수 있다.
```jsx
function myComponent() {
    useEffect(() => {
        console.log('이 부분이 componentDidMount 처럼 동작합니다.');
    }, []) // 빈 배열을 넣어야 마운트 시에 한 번만 실행됩니다.

    return <div>Hello Moles!</div>
}
```

#### `componentDidUpdate`
리렌더링 작업이 완료된 직후 실행되는 메서드이다.<br/>
의존성 배열에 관찰하고자 하는 특정 state, props를 추가하여 구현한다.
```jsx
function myComponent() {
    const [state, setState] = useState();
    
    useEffect(() => {
        console.log('이 부분이 componentDidUpdate 처럼 동작합니다.');
    }, [state]) // state가 변경될 때마다 실행됩니다.

    return <div>Hello Moles!</div>
}
```

#### `componentWillUnmount`
컴포넌트가 사라지기 직전에 호출되는 메서드이다.<br/>
useEffect의 return 문에 언마운트 시 실행하고 싶은 작업을 작성한다.
```jsx
function myComponent() {
    useEffect(() => {
        return () => {
            console.log('이 부분이 componentWillUnmount 처럼 동작합니다.');
        };
    }, []) // 빈 배열을 넣으면 cleanup 함수는 언마운트 시에 실행됩니다.

    return <div>Hello Moles!</div>
}
```


#### `shouldComponentUpdate`
컴포넌트를 리렌더링 할지 말지(= 불필요한 렌더링 방지) 결정하는 메서드이다.<br/>
useMemo를 사용하여 컴포넌트를 캐싱한다.
```jsx
const MyComponent = React.memo(({name}) => {
    return <div>{name}</div>
})

function ParentComponent() {
    const [name, setName] = useState('yubin');
    const memoizedComponent = useMemo(() => <MyComponent name={name}/>, [name]);

    return (
        <div>
            {memoizedComponent}
        </div>
    )
}
```

#### `getDerivedStateFromProps`
props 또는 state가 변경되었을 때 render 메서드 이전에 실행된다.<br/>
props에 따라 state가 업데이트 되어야 할 때 사용한다.
useState와 useEffect를 조합하여 구현한다.

```jsx
function MyComponent({ someProp }) {
    const [state, setState] = useState(someProp);

    useEffect(() => {
        setState(someProp); // someProp이 변경되면 state도 변경됩니다.
    }, [someProp]);

    return <div>{state}</div>
}
```

`useEffect`를 적절하게 사용하면 클래스형 컴포넌트의 라이프사이클 메서드를 대부분 대체할 수 있구나...<br/>
불필요한 렌더링과 부수 효과로 최소화 해야된다고 권장되는 경우도 있지만, 불가피 한 경우에 유용하게 잘 쓰먼 좋겠다.