# `const` vs `function`

JavaScript에서 함수를 정의할 때 사용하는 대표적인 두 가지 키워드이다.
<br>각 키워드의 개념과 어떤 차이점을 갖는지 간단히 짚어보자.

## `const`

`const` 키워드를 사용하여 함수를 선언하는 것은 **함수 표현식**을 사용하는 것이다.
<br>`const`의 역할은 변수 선언인데, 이 **변수에 익명 함수를 할당하는 방식**으로 함수를 정의한다.

```javascript
const niceToMeetYou = function () {
  console.log("Hello Moles!");
};

niceToMeetYou(); // "Hello Moles!"
```

## `function`

`function` 키워드를 사용하여 함수를 선언하는 것은 **함수 선언식**이라고 부른다.

```javascript
function niceToMeetYou() {
  console.log("Hello Moles!");
}

niceToMeetYou(); // "Hello Moles!"
```

## `const` vs `function`

세 가지 차이점을 갖는다.

### 1. 호이스팅

자바스크립트 인터프리터가 코드를 읽는 방식으로,
<br>스코프(함수) 안에 존재하는 모든 **선언**들을 해당 스코프(함수)의 최상단으로 끌어올리는 것이다.

`const`: **일시적 사각지대(TDZ, Temporal Dead Zone)에 존재** - 선언 전에 접근 불가능

```javascript
niceToMeetYou(); // ❗Uncaught ReferenceError: Cannot access 'niceToMeetYou' before initialization

const niceToMeetYou = function () {
  console.log("Hello Moles!");
};
```

`function`: **호이스팅 ⭕** - 코드 내 어디서든 호출 가능

```javascript
niceToMeetYou(); // "Hello Moles!"

function niceToMeetYou() {
  console.log("Hello Moles!");
}
```

### 2. Immutable vs Mutable

`const`: **Immutable** - 함수 재할당 ❌.

```javascript
const niceToMeetYou = function () {
  console.log("Hello Moles!");
};

niceToMeetYou(); // "Hello Moles!"

niceToMeetYou = function () {
  console.log("Hi Moles!");
}; // ❗Uncaught TypeError: Assignment to constant variable.
```

`function`: **Mutable** - 함수 재할당 ⭕. 의도하지 않게 함수가 변경될 가능성이 있다.

```javascript
function niceToMeetYou() {
  console.log("Hello Moles!");
}

niceToMeetYou(); // "Hello!"

// 다른 함수를 같은 이름으로 선언
function niceToMeetYou() {
  console.log("Hi Moles!");
}

niceToMeetYou(); // "Hi Moles!"
```

### 3. 코드 가독성 및 유지보수성

`const`: 코드를 깔끔하게 작성할 수 있어 가독성 및 유지보수 측면에서 유리하다.

```javascript
const add = (a, b) => a + b;

const multiply = (a, b) => a * b;

console.log(add(2, 3)); // 5
console.log(multiply(2, 3)); // 6
```

`function`: 전통적인 방법으로, 호이스팅이 필요한 상황에서 유용하다.

```javascript
function add(a, b) {
  return a + b;
}

function multiply(a, b) {
  return a * b;
}

console.log(add(2, 3)); // 5
console.log(multiply(2, 3)); // 6
```

## 함수 호이스팅

그렇다면 호이스팅은 언제 필요한 걸까? 예시를 통해 맛만 그냥 살짝 보도록 하자.

### 1. Top-Down 코딩 스타일

코드의 논리적인 순서를 유지하면서도, 함수 정의를 코드의 아래쪽에 위치시킬 수 있다.
<br>이렇게하면 코드의 흐름을 보다 직관적으로 보이도록 배치할 수 있게 된다.

```javascript
function main() {
  initializeApp();
  handleUserInput();
  renderUI();
}

function initializeApp() {
  console.log("App initialized.");
}

function handleUserInput() {
  console.log("Handling user input.");
}

function renderUI() {
  console.log("Rendering UI.");
}

main();
```

### 2. Self-contained 모듈 작성

복잡한 프로그램에서는 코드를 모듈화하여 관리하는 것이 일반적이다.
<br>이때, 모듈의 진입점(Entry Point)을 맨 위에 두고, 그 아래에 모듈에서 사용하는 여러 보조 함수를 정의할 수 있다.
<br>아래 코드에서 `runApp` 함수는 모듈의 진입점 역할, `setup`과 `start` 함수는 보조 역할이다.
<br>함수 호이스팅 덕분에 `runApp` 함수가 먼저 호출되어도 문제없이 동작한다.

```javascript
(function () {
  runApp();

  function runApp() {
    setup();
    start();
  }

  function setup() {
    console.log("Setup complete.");
  }

  function start() {
    console.log("App started.");
  }
})();
```

### 3. 재귀 함수의 선언 전 호출

자신을 호출하는 함수인 재귀 함수의 경우, 함수가 선언되기 전에 호출되는 경우가 잦다.
<br>이를 함수 호이스팅이 지원해준다.

```javascript
console.log(factorial(5)); // 120

function factorial(n) {
  if (n === 1) return 1;
  return n * factorial(n - 1);
}
```

### 4. 테스트 및 디버깅

어떤 함수가 여러 번 호출되는데, 그 함수의 위치가 코드의 맨 아래쪽에 있을 때.
<br>함수 호이스팅을 이용하여 함수 선언 위치의 변경 없이 코드 테스트 또는 디버깅을 할 수 있다.

<br>즉, 함수 호이스팅은 코드의 흐름을 직관적으로 구성하고, 함수 선언의 위치에 구애받지 않고 함수를 호출할 수 있도록 하여 **코드의 가독성과 유지보수성을 높이는 데 활용**된다. 때문에, 함수의 호출 순서가 중요한 경우나, 모듈화된 코드에서 진입점을 먼저 보여주고 싶은 경우에 유용하게 사용할 수 있다. 함수를 정의하는 두 방법의 큰 차이점 중 하나인 호이스팅까지 알아보며 요번 공부를 마친다.
