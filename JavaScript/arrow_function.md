# 화살표 함수

이전에는 `function` 키워드를 사용해서 함수를 선언했다. <br/>ES6에서 등장한 화살표 함수는 `=>`를 사용하여 보다 간략한 방법으로 함수를 선언한다.

## 화살표 함수의 호출

화살표 함수는 익명 함수로만 사용할 수 있다. 따라서 화살표 함수를 호출하기 위해서는 함수 표현식을 사용한다.

```javascript
const sayHello = () => console.log("hello mole~");
sayHello(); // hello mole~
```

## 화살표 함수의 this

일반 함수(function)와 화살표 함수(=>)의 가장 큰 차이점은 `this`이다.

function으로 선언한 함수가 메소드로 호출되냐 함수 자체로 호출되냐에 따라 동적으로 this가 바인딩되는 반면, 화살표 함수는 선언될 시점에서의 상위 스코프가 this로 바인딩된다.

## 화살표 함수를 사용하면 안되는 경우

### 1. 메소드

메소드로 정의한 화살표 함수의 this는 상위 스코프인 전역 객체 window를 가리킨다.

```javascript
const badMole = {
  name: 'yuna',
  sayHi: () => console.log(`Hi ${this.name}`);
};

badMole.sayHi(); // Hi undefined ❌

// ES6의 축약 메서드 표현 (권장)
const goodMole = {
  name: 'yubin',
  sayHi() {
    console.log(`Hi ${this.name}`);
  }
}

goodMole.sayHi(); // Hi yubin ⭕️
```

<br/>

### 2. prototype

화살표 함수로 정의된 메서드를 prototype에 할당하는 경우도 1번과 동일한 문제가 발생한다.

```javascript
const mole = {
  name: "yubin",
};

// 화살표 함수
Object.prototype.sayHi = () => console.log(`Hi ${this.name}`);

mole.sayHi(); // Hi undefined ❌

// 일반 함수
Object.prototype.sayHi = function () {
  console.log(`Hi ${this.name}`);
};

mole.sayHi(); // Hi yubin ⭕️
```

<br/>

### 3. 생성자 함수

화살표 함수는 생성자 함수로 사용할 수 없다.
<br/>생성자 함수는 prototype이라는 속성을 가지며 이 속성이 가리키는 프로토타입 객체의 constructor를 사용한다. 하지만 화살표 함수는 이를 가지고 있지 않다.

```javascript
const Foo = () => {};

console.log(Foo.hasOwnProperty("prototype")); // false

const foo = new Foo(); // TypeError: Foo is not a constructor
```

<br/>

### 4. addEventListener 함수의 콜백 함수

addEventListener 함수의 콜백 함수를 화살표 함수로 정의하면 this는 전역 객체 window를 가리킨다.
<br/>addEventListener 함수의 콜백 함수 내에서 this를 사용하는 경우, 일반 함수를 사용해야 한다. 그 때의 this는 이벤트 리스너에 바인딩된 요소(currentTarget)를 가리킨다.

```javascript
button.addEventListener("click", () => {
  console.log(this === window); // true ❌
  this.innerHTML = "Clicked Button";
});

button.addEventListener("click", function () {
  console.log(this === window); // false ⭕️
  this.innerHTML = "Clicked Button";
});
```

[참고]

- [Arrow funtion | PoiemaWeb](https://poiemaweb.com/es6-arrow-function)
