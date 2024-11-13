# 호이스팅

호이스팅(Hoisting)이란 함수 내의 변수 및 함수 선언을 각 유효 범위의 최상단으로 끌어 올려주는 JavaScript의 특징이다.

실제로 코드가 끌어올려지는 것은 아니고, JavaScript 인터프리터가 내부적으로 끌어올려 처리한다.

```JavaScript
console.log(mole); // undefined
var mole = "두더지";
```

mole을 선언하기 전에 로그를 찍었지만, 에러가 발생하지 않는 것을 볼 수 있다.

```JavaScript
var mole;
console.log(mole);
mole = "두더지";
```

내부적으로 `var mole`을 상단으로 끌어올려 처리했기 때문이다. <br/>
정확하게는 var 키워드로 선언한 변수일 경우 오류 없이 동작한다.

모든 선언(function, var, let, const, class)은 호이스팅되며, <br/> `var` 선언은 선언과 동시에 undefined로 초기화되지만 `let`, `const` 선언은 초기화되지 않은 Temporal Dead Zone(TDZ) 상태로 유지된다.

## JavaScript의 변수 생성 단계

JavaScript에서는 총 3단계에 걸쳐 변수를 생성한다.

1. **선언** : 스코프와 변수 객체가 생성된다. 스코프가 변수 객체를 참조한다. _(초기화 전 까지는 TDZ 상태)_
2. **초기화** : 변수 객체 값을 위한 공간을 메모리에 할당한다. 이때 그 공간에는 undefined가 할당된다.
3. **할당** : 변수 객체에 값을 할당한다.

## 변수 호이스팅 : var, let, const

`var`는 선언과 동시에 undefined로 초기화가 이루어진다. <br/> 그러나 `let`, `const`는 선언만 될 뿐, TDZ 상태에 들어가게 된다.
TDZ 상태의 변수를 참조하면 참조 에러가 발생한다.

```JavaScript
console.log(mole); // undefined
var mole = "mole with var keyword"
```

```JavaScript
console.log(mole); // ReferenceError: mole is not defined ...
let mole = "mole with let keyword"
```

`var` 키워드로 선언된 변수만 호이스팅이 되는 것이 아니라, <br/> 모든 선언이 JavaScript에서 호이스팅되지만 `let`, `const`로 선언은 초기화되지 않은 상태로 유지되는 것이다.

## 함수 호이스팅

함수 선언문의 경우, 호이스팅으로 인해 선언 전에 함수를 호출할 수 있다.<br/>
하지만 함수 표현식은 변수 호이스팅의 규칙을 따르기 때문에, 선언 전에 호출하면 참조 에러가 발생한다.

```JavaScript
sayHello('shun'); // hello shun!
function sayHello(name) {
  console.log(`hello ${name}!`);
}

sayBye('yubin'); // TypeError: sayBye is not a function
var sayBye = function(name) => {
  console.log(`bye ${name}`);
}
```

## 마치며..

호이스팅은 선언 위치를 신경쓰지 않고 필요한 곳에서 변수나 함수를 자유롭게 사용하기 위해 만들어졌다.<br/>
그러나 때로는 의도치 않은 결과를 초래할 여지가 있어서 호이스팅을 의도적으로 사용하는 경우가 아니라면 `let`, `const` 키워드를 사용하는 것을 권장한다.
<br/>

[참고]

- [[JavaScript] 변수와 함수 호이스팅(Hoisting)에 대해 알아보자](https://velog.io/@1nthek/JavaScript-%EB%B3%80%EC%88%98%EC%99%80-%ED%95%A8%EC%88%98-%ED%98%B8%EC%9D%B4%EC%8A%A4%ED%8C%85Hoisting%EC%97%90-%EB%8C%80%ED%95%B4-%EC%95%8C%EC%95%84%EB%B3%B4%EC%9E%90)
- [자바스크립트의 호이스팅(Hoisting) 이해하기](https://f-lab.kr/insight/understanding-javascript-hoisting)
