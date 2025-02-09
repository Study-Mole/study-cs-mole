# call, apply, bind

`call()`, `apply()`, `bind()`는 JavaScript에서 `this`를 명확하게 제어할 때 유용한 메서드이다.

## `this`를 명확히 해야 하는 이유

```javascript
const myObject = {
  value: 10,
  getValue() {
    console.log(this.value);
  },
};

myObject.getValue(); // 10
```

단순하게 객체의 메소드로 실행하면 `this`는 명확하다.

그러나 아래처럼 `getValue` 함수를 다른 변수에 대입해서 새로운 함수를 만들 때 문제가 생긴다.

```javascript
const myObject = {
  value: 10,
  getValue() {
    console.log(this.value);
  },
};

const boundFunction = myObject.getValue;
boundFunction(); // undefined
```

`boundFunction` 이라는 변수에 `getValue` 함수를 할당할 때 함수의 참조만을 전달하는데, 그렇게 되면 함수 자체가 단독으로 호출되어 `this`는 전역 객체(브라우저: Window, Nodejs: global)를 참조한다.

따라서 함수 내부의 `this`는 전역 객체인 `window`를 가리키고, `this.value`가 `window.value`를 참조하게 된다.
<br/>그런데 `window` 전역객체에는 `value` 라는 항목이 없으니 `undefined`를 출력하는 것이다.

이렇게 `this`가 명확하지 않음으로써 발생하는 문제를 `call`, `apply`, `bind`로 해결할 수 있다.

참고로 `this`의 모호성에 대해 [화살표 함수편](../JavaScript/arrow_function.md)에서 간단히 언급하고 있으니, 참고해봐도 좋겠다!

## call()

`call()` 메서드는 함수를 호출하면서 첫 번째 인자로 `this`를 지정하고, 이후의 인자를 개별적으로 전달한다.

```javascript
function greet(greeting, punctuation) {
  console.log(greeting + ", " + this.name + punctuation);
}

const person = { name: "Moles" };

greet.call(person, "Hello", "!"); // Hello, Moles!
```

`call()`을 사용할 땐 첫 번째 인자로 `this`를 지정하고, 나머지 인자를 각각 전달한다.
<br/>또한 호출 시 즉시 실행된다는 특징을 갖는다.

## apply()

`call()`과 거의 동일하다.
<br/>다만 인자를 개별적으로 넘기는 대신 **배열로 전달**해야 한다는 점에서 차이가 있다.

```javascript
greet.apply(person, ["Hi", " :)"]); // Hi, Moles :)
```

## bind()

`call()` 그리고 `apply()`와 달리, 즉시 실행되지 않고 **새로운 함수를 반환**한다.

```javascript
const boundGreet = greet.bind(person, "Hey", "!");
boundGreet(); // Hey, Moles!
```

즉, `this`를 지정하지만 함수를 즉시 실행하지 않는다.
<br/>반환한 새로운 함수는 나중에 실행하거나, 뒤에 `()`를 붙여 즉시 실행시킬 수 있다.

```javascript
const boundGreet = greet.bind(person, "Hey", "!")(); // Hey, Moles!
```

## call vs apply vs bind

세 메서드의 차이점을 표로 정리하면 아래와 같다.

| 메서드  | 즉시 실행 여부       | 인자 전달 방식 | 반환 값        |
| ------- | -------------------- | -------------- | -------------- |
| `call`  | O (즉시 실행)        | 개별 전달      | 함수 실행 결과 |
| `apply` | O (즉시 실행)        | 배열 전달      | 함수 실행 결과 |
| `bind`  | X (새로운 함수 반환) | 개별 전달      | 새로운 함수    |

[참고]

- [자바스크립트 bind, call, apply 완벽 이해](https://mycodings.fly.dev/blog/2024-01-01-all-about-javascript-bind-call-apply)
