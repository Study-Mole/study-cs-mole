# JavaScript

- [화살표 함수](#화살표-함수)
- [함수 선언형과 함수 표현식](#함수-선언형과-함수-표현식)
- [`==`과 `===`의 차이점](#과-의-차이점)
- [구조 분해 할당 (Destructing assignment)](#구조-분해-할당-destructing-assignment)

## 화살표 함수

## 함수 선언형과 함수 표현식

## `==`과 `===`의 차이점

## 구조 분해 할당 (Destructing assignment)

### 구조 분해 할당이란?

- 배열이나 객체의 속성을 해체하여 그 값을 개별 변수에 담을 수 있게 하는 JavaScript 표현식입니다.

### 맛보기

코드로 대강 파악해보게요.

```javascript
let a, b, rest;
[a, b] = [10, 20];
console.log(a); // 10
console.log(b); // 20

[a, b, ...rest] = [10, 20, 30, 40, 50];
console.log(a); // 10
console.log(b); // 20
console.log(rest); // [30, 40, 50]

({a, b}} = { a: 10, b: 20});
console.log(a); // 10
console.log(b); // 20

({a, b, ...rest} = {a: 10, b: 20, c: 30, d: 40});
console.log(a); // 10
console.log(b); // 20
console.log(rest); // {c: 30, d: 40}
```

### 기본 변수 할당

배열 먼저 살펴볼게요. 기본적인 변수 할당 예제입니다.

```javascript
let x = [1, 2, 3, 4, 5];
let [y, x] = x;
console.log(y); // 1
console.log(z); // 2
```

### 선언에서 분리한 할당

변수 선언이 분리되어도 구조 분해로 값을 할당할 수 있어요.

```javascript
let a, b;
[a, b] = [1, 2];
console.log(a); // 1
console.log(b); // 2
```

### 기본값

분해한 값이 `undefined` 일 때, 기본값이 없으면 그대로 `undefined`지만 있으면 그 값을 대신 사용합니다.

```javascript
let a, b;
[a = 5, b = 7] = [1];
console.log(a); // 1
console.log(b); // 7
```

```javascript
let a, b;
[a = 5, b] = [1];
console.log(a); // 1
console.log(b); // undefined
```

```javascript
let a, b;
[a = 5, b] = [];
console.log(a); // 5
console.log(b); // undefined
```

### 값 교환

하나의 구조 분해 표현식으로 두 변수의 값을 교환할 수 있습니다. 구조 분해 할당 개념을 찾아보게 된 계기이기도 해요. 왕싱기

```javascript
let a = 1;
let b = 2;

[a, b] = [b, a];
console.log(a); // 2
console.log(b); // 1
```

### 함수가 반환한 배열 분석

함수도 구조 분해 할당이 가능하다는 의미입니다.

```javascript
function f() {
  return [1, 2];
}

var a, b;
[a, b] = f();
console.log(a); // 1
console.log(b); // 2
```

### 일부 반환값 무시

필요없는 값은 건너뛸 수 있어요.

```javascript
function f() {
  return [1, 2, 3];
}

var [a, , b] = f();
console.log(a); // 1
console.log(b); // 3
```

싸그리 무시할 수도 있어요. 근데 이런 건 어디에 써먹을 수 있을까요?

```javascript
var [, ,] = f();
```

### 나머지 할당하기

[맛보기](#맛보기)의 rest와 같습니다. 나머지 요소는 항상 선언된 변수 중 가장 뒤에 위치해야 해요.

```javascript
var [a, ...b] = [1, 2, 3, 4];
console.log(a); // 1
console.log(b); // [2, 3, 4]

var [...c, d] // SyntaxError 가 발생합니다
```

### 정규 표현식과 함께 활용하기

정규 표현식의 exec() 메서드는 일치하는 부분을 찾으면 그 문자열에서 정규식과 일치하는 부분 전체를 배열[0]에, 그 뒤에 정규식에서 ()로 묶인 각 그룹과 일치하는 부분을 포함하는 배열을 반환합니다. 입맛대로 잘 쪼갠 뒤 구조 분해 할당으로 원하는 부분만 챙겨올 수 있겠죠.

```javascript
function eGaeMoya(url) {
  let parsedURL = /^(\w+)\:\/\/([^\/]+)\/(.*)$/.exec(url);
  if (!parsedURL) return false;
  console.log(parsedURL); // ["https://developer.mozilla.org/en-US/Web/JavaScript", "https", "developer.mozilla.org", "en-US/Web/JavaScript"]

  let [, protocol, fullhost, fullpath] = parsedURL;
  return protocol;
}

console.log(eGaeMoya("https://developer.mozilla.org/en-US/Web/JavaScript")); // "https"
```

### 기본 할당

이번엔 객체 구조 분해입니다.

```javascript
let o = { p: 42, q: true };
let { p, q } = o;
console.log(p); // 42
console.log(q); // true
```

### 선언 없는 할당

배열에서 선언 없는 할당과 같은 방식으로, 잘 동작합니다. 배열 구조 분해와 다르게 할당문을 둘러싼 `()`가 필요합니다. 이유는

```javascript
let a, b;
({ a, b } = { a: 1, b: 2 });
```

### 새로운 변수명 할당하기

```javascript

```

### 기본값

```javascript

```

### 새로운 변수명 할당 + 기본값

```javascript

```

### 함수 매개변수의 기본값 (ES5)

```javascript

```

### 함수 매개변수의 기본값 (ES2015)

```javascript

```

### 중첩된 객체/배열의 구조 분해

```javascript

```

### for of 반복문과 구조 분해

```javascript

```

### 함수 매개변수로 전달된 객체에서 필드 해체하기

```javascript

```

### 계산된 속성명과 구조 분해

```javascript

```

### 객체 구조 분해에서 rest

```javascript

```

### 속성 이름이 유효한 JavaScript 식별자명이 아닐 때

```javascript

```

### 참고

- [Mdn](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Operators/Destructuring_assignment)
