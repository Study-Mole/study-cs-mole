# `type` vs `interface`

`type`과 `interface`는 <b>객체를 타이핑하기 위해 사용하는 키워드</b>이다. (= 사용 목적이 같다)

```
type Object = { // 변수를 선언하는 것과 비슷
    title: string;
    description: string;
};

interface IObject { // interface라는 이름처럼 객체 타입을 선언
    title: string;
    description: string;
};

const object1: Object = {...};
const object2: IObject = {...};
```

위와 같이 객체 타입을 정의할 때는 큰 차이가 없다.<br/>코드에서도 볼 수 있는 것처럼 `type`은 `=` 키워드를 사용하여 타입 정보를 할당하는 개념이고,
`interface`는 타입 자체를 선언하는 느낌이다.

## 차이점

### 1. `type` 키워드는 객체 타입 뿐만 아니라 모든 유형의 타입을 정의할 수 있다.

- 원시 타입, 튜플 타입, 함수 타입, 유니온 타입, 매핑된 타입
- 아래와 같은 타입들은 `interface`로는 정의할 수 없다.

```
type A = string; // primitive
type B = [string, number, boolean]; // tuple
type C = 'apple' | 'potato'; // union
type D = { [K in keyof T] : string }; // mapped
```

### 2. 타입을 확장하는 방법도 서로 다르다.

- `type`은 `&` 키워드를, `interface`는 `extends` 연산자를 사용한다.

```
type B = A & {
    ...
}

interface B extends A
```

- `interface`는 선언적 확장이 가능하지만 `type`은 그렇지 않다.

```
interface A {
    name: string;
    age: number;
}

// 선언적 확장
interface A {
    email: string;
}

type B = {
    name: string;
    age: number;
}

// ❗️Error: Duplicate indentifier 'B'
type B = {
    email: string;
}
```

<br/>
결론적으로 타입은 모든 타입을 정의할 수 있고 확장도 가능하기에 인터페이스를 대부분 커버할 수 있다. 그러나 확장면에서는 인터페이스가 더 유연하다. `type`과 `interface`중 어떤 키워드를 사용할지는 개인의 취향이나 팀의 컨벤션에 따라 다를 수 있다.
