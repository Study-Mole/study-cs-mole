# `new` 연산자

사용자 정의 객체 타입 또는 내장 객체 타입의 인스턴스를 생성하는 연산자이다.
<br>아래와 같은 형태를 갖는다.

```javascript
new constructor[[arguments]]();
```

- `constructor`: 객체 인스턴스의 타입을 기술하는 함수
- `arguments`: `constructor`와 함께 호출될 값 목록

<br>그러니까 대충 이런 식으로 쓰는거다.

```javascript
function Mole(name, age, height) {
  this.name = name;
  this.age = age;
  this.height = height;
}

const yubin = new Mole("yubin", 25, 165);

console.log(yubin.height); // 165
```

## 사용자 정의 객체

`new` 연산자는 함수(생성자 함수)를 통해 새로운 객체를 생성할 때 사용한다.
<br>`new`를 통해 생성된 객체는 해당 함수의 `prototype`을 상속받는다.
<br>이 때, 생성자 함수 내에서 `this`는 생성된 새 객체를 가리킨다.

객체를 생성하는 과정을 단계별로 정리하면 다음과 같다.

- 새 객체 생성: `new`는 먼저 `Mole.prototype`을 상속하는 새로운 객체를 만든다.
- 생성자 함수 호출: 생성자 함수 `Mole`이 호출되고, 그 안의 `this`는 새로 만들어진 객체를 가리킨다. 생성자 함수에서 속성 등을 설정할 수 있다.
- 객체 반환: 생성자 함수가 명시적으로 객체를 반환하지 않으면, 기본적으로 생성된 객체를 반환한다.

<br>

```javascript
function Mole(name, age) {
  this.name = name;
  this.age = age;
  this.describe = function () {
    return `This mole is ${this.name}. And she is ${this.age}.`;
  };
}

const myMole = new Mole("yubin", "25");
console.log(myMole.describe()); // "This mole is yubin. And she is 25."
```

위 코드에서 `new Mole('yubin', '25');`가 실행되면, `Mole.prototype`을 상속하는 새 객체가 생성된다.
<br>`Mole` 함수가 호출되며, `this`는 새로 만들어진 객체를 가리킨다. `this.name`과 `this.age` 속성에 값이 할당된다.
<br>생성된 객체가 `myMole` 변수에 할당된다.

<br>

## 속성이 또 다른 객체일 때

```javascript
function Mole(mind) {
  this.mind = mind;
}

function Person(sex, name, mole) {
  this.sex = sex;
  this.name = name;
  this.mole = mole; // Mole 객체를 속성으로 가짐
  this.describe = function () {
    return `${this.sex} is ${this.name} with ${this.mole.mind} mind.`;
  };
}

let myMole = new Mole("strong"); // Mole 객체 생성
let myPerson = new Person("She", "yuna", myMole); // Person 객체 생성, Mole 포함

console.log(myPerson.describe()); // "She is yuna with strong mind."
```

1. `Mole`이라는 객체가 따로 있고, `Person` 객체는 `Mole` 객체를 속성으로 가질 수 있다.
2. `myMole`이라는 `Mole` 객체를 만들고, 이를 `myPerson`이라는 `Person` 객체의 속성으로 전달한다.

<br>

## 이전에 정의된 객체에 속성을 추가할 때

```javascript
function Mole() {}
yuna = new Mole();
yubin = new Mole();

console.log(yuna.run); // undefined

Mole.prototype.run = "Tancheon";
console.log(yuna.run); // Tancheon

yubin.run = "Hokey";
console.log(yubin.run); // Hokey

console.log(yuna.__proto__.run); //Tancheon
console.log(yubin.__proto__.run); //Tancheon
console.log(yuna.run); // Tancheon
console.log(yubin.run); // Hokey
```

공유 속성을 추가할 때는 `.prototype` 속성을 사용한다. 이는 객체 타입의 인스턴스 하나에만 적용되는 것이 아니라 이 함수로 생성하는 모든 객체와 공유하는 속성을 정의하게 된다.

이와 다르게 한 객체에 추가한 속성은 다른 객체들에게는 영향을 주지 않는다.
<br>동일한 타입의 모든 객체들에게 새로운 속성을 추가하려면, `Mole` 객체 타입의 정의에 이 속성을 추가해야한다.

이렇게 `new` 연산자를 통해 다양한 객체를 생성하고, 속성으로 다른 객체를 포함하는 방식으로 객체 지향 프로그래밍을 구현할 수 있다.

[참고]

- [new operator | mdn web docs](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Operators/new)
