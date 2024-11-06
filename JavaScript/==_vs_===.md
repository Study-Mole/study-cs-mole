## `==` vs `===` (Equality Operators)

각각 느슨한 동등 비교와 엄격한 동등 비교를 위해 사용되는 연산자이다.
<br>비슷해 보이지만 사실은 매우 다른 두 개념을 살펴보자.

### `===` (Strict Equality Operator)

엄격한 동등성, 즉 타입과 값이 모두 같은지 비교할 때 사용한다.
<br>`==` 연산자보다 간단한 논리를 거친 결과를 반환하므로 보다 먼저 살펴보자.

같은 타입과 같은 값을 가진 것 사이의 비교이다.
<br>모두 `true`를 반환한다.

```javascript
5 === 5;
// true (모두 숫자, 같은 값)

"hello world" === "hello world";
// true (모두 문자열, 같은 값)

true === true;
// true (모두 불리언, 같은 값)
```

바로 두 번째 예제로 넘어가보자.

```javascript
77 === "77";
// false (숫자와 문자열, 같은 값)

"cat" === "dog";
// false (모두 문자열, 다른 값)

false === 0;
// false (불리언과 숫자, 다른 값)
```

즉, `===`은 타입과 값 중 하나라도 다르면 두 값이 같지 않다고 판단하여 `false`를 반환한다.
<br>아주 직관적인 비교 연산자라고 할 수 있겠다.

### `==` (Abstract Equality Operator)

느슨한 동등성을 비교할 때 사용한다.
<br>강제 형변환(type coercion)을 통해 피연산자들을 공통 타입으로 만든 뒤에, 비교 연산을 수행한다.

`===` 연산자와 비교하여 살펴보자.

```javascript
77 === "77"; // false
77 == "77"; // true

false === 0; // false
false == 0; // true
```

이러한 차이는 강제 형변환에서 나타난다.
<br>자바스크립트가 값을 동등한 타입으로 변환한 후에 값을 비교하기 때문이다.
<br>문자열 `'77'`은 숫자 `77`로, 숫자 `0`은 `falsy`값 `false`로 변환된 것이다.

### `Falsy` 값

`false == 0`이 성립하는 이유는 자바스크립트에서 `0`이란 값이 `falsy` 값이기 때문이다.
<br>`0` 이외에 `false`, `""`, `null`, `undefined`, `NaN` 값이 자바스크립트에서 `falsy` 값에 해당한다.

이번 기회에 아래 비교 규칙들을 암기해두도록 하자.
<br>개인적으론 매번 헷갈리는 것들이다.

`NaN`값이 가장 쉽다.
<br>굉장히 부정적이라고 볼 수 있겄다.

```javascript
false == 0; // true
0 == ""; // true
"" == false; // true

null == null; // true
undefined == undefined; // true
null == undefined; // true

NaN == null; // false
NaN == undefined; // false
NaN == NaN; // false
```

이제 두 변수가 같은지 확인하는, 우리가 일반적으로 원하는 동등 비교 시에는 `===` 연산자를 사용하기로 하자.

[참고]

- [JavaScript — Double Equals vs. Triple Equals](https://codeburst.io/javascript-double-equals-vs-triple-equals-61d4ce5a121a)

<br/>
