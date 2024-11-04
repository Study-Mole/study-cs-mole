## 클로저 (Closure)
- [클로저의 활용](#클로저의-활용)

> A Closure is the combination of a function and the lexical environment within which that function was declared.<br/> _**클로저는 함수와 그 함수가 선언되었을 때의 렉시컬 환경과의 조합이다.**_

너무나도 난해하므로 예제부터 본다.

```javascript
function outerFunc() {
  var x = 10;
  var innerFunc = function () {
    console.log(x);
  };
  innerFunc();
}

outerFunc(); // 10
```

innerFunc는 자신을 포함하고 있는 외부함수 outerFunc의 변수 x에 접근했다.<br/>
스코프는 호출할 때가 아니라 함수를 어디서 선언했는지에 따라 결정된다. 이를 **렉시컬 스코핑(Lexical Scoping)** 이라 한다.

innerFunc는 자신이 속한(= 선언된 곳) 렉시컬 스코프(자신의 스코프, outerFunc, 전역)를 참조할 수 있다.

innerFunc이 호출될 때 스코프 체인은 전역 객체, outerFunc의 활성 객체, innerFunc 자기 자신의 스코프를 가리키는 활성 객체를 순차적으로 바인딩한다. 스코프 체인이 바인딩한 객체가 렉시컬 스코프의 실체이다.

변수 x를 찾는 경우 스코프 체인을 따라 다음과 같은 동작을 한다.

1. innerFunc 함수 스코프에서 변수 x를 검색한다.
2. (1번이 실패한 경우) outerFunc 함수 스코프에서 변수 x를 검색한다.
3. (2번이 실패한 경우) 전역 스코프에서 변수 x를 검색한다.

<br/>

이번엔 outerFunc이 innerFunc을 반환하도록 코드를 수정해보자.

```javascript
function outerFunc() {
  var x = 10;
  var innerFunc = function () {
    console.log(x);
  };
  return innerFunc;
}

/**
 *  함수 outerFunc를 호출하면 내부 함수 innerFunc가 반환된다.
 *  그리고 함수 outerFunc의 실행 컨텍스트는 소멸한다.
 */

var inner = outerFunc();
inner(); // 10
```

outerFunc은 innerFunc을 반환하고 소멸했지만 놀랍게도 innerFunc은 outerFunc의 x를 참조해온다.<br/>
자신을 포함하고 있는 외부함수보다 내부함수가 더 오래 유지되는 경우, <u>**외부함수 밖에서 내부함수가 호출되더라도 외부함수의 지역 변수에 접근할 수 있는데 이러한 함수를 클로저(Closure)라고 한다.**</u>

<br/>

난해했던 정의로 다시 돌아가본다.

> A Closure is the combination of a function and the lexical environment within which that function was declared.<br/> _**클로저는 함수와 그 함수가 선언되었을 때의 렉시컬 환경과의 조합이다.**_

"함수"는 innerFunc을 말하는 것이고 "렉시컬 환경"은 innerFunc이 정의되었던 스코프를 의미한다.

즉, **클로저는 반환된 내부함수가 자신이 선언되었을 때의 환경을 기억하여 자신이 선언된 환경 밖에서 호출되어도 그 환경에 접근할 수 있는 함수를 말한다.**

## 클로저의 활용

### 상태 유지 및 전역 변수 사용 억제

현재 상태를 기억하고 변경된 최신 상태를 유지하는 것은 클로저의 가장 큰 장점이다.

```javascript
var btn = document.querySelector(".btn");

// toggle은 즉시 실행 함수
var toggle = (function () {
  var isShow = false;

  // 1. 클로저 반환
  return function () {
    // 2. 상태 변경
    isShow = !isShow;
  };
})();
// 3. 이벤트 프로퍼티에 클로저 할당
btn.onclick = toggle;
```

1. 즉시 실행 함수는 함수를 반환하고 즉시 소멸한다. toggle이 반환하는 함수는 자신이 생성되었을 때의 렉시컬 환경에 속한 isShow를 기억하는 클로저다.
2. 클로저를 이벤트 핸들러로서 이벤트 프로퍼티에 할당했다. 이를 제거하지 않는 한 렉시컬 환경의 isShow는 소멸하지 않고 현재 상태를 기억한다.
3. 버튼을 클릭하면 클로저가 호출된다. 이 때 isShow의 값이 토글되며 isShow는 클로저에 의해 참조되고 있기 때문에 유효하고 최신 상태를 계속해서 유지한다.

만약 javascript에 클로저가 없다면 이 상태를 유지하기 위해 전역 변수를 사용해야 한다.<br/>

하지만 전역 변수는 누구나 접근 가능하므로 의도치 않은 값의 변경이 일어날 수 있다. 상태 변경이나 가변 데이터를 피하고 불변(immutability)을 지향하는 함수형 프로그래밍에서 side effect를 최대한 억제하여 오류를 피하기 위해 클로저는 적극적으로 사용된다.

### 정보의 은닉

```javascript
function Counter() {
  this.name = "cute mole";
  var counter = 0;

  this.increase = function () {
    return [this.name, ++counter];
  };

  this.decrease = function () {
    return [this.name, --counter];
  };
}

const counter = new Counter();

console.log(counter.name); // cute mole -> 프로퍼티는 인스턴스를 통해 외부에서 접근 가능
console.log(counter.increase()); // [ 'cute mole', 1 ]
console.log(counter.decrease()); // [ 'cute mole', 0 ]
```

생성자 함수 Counter는 increase, decrease 메서드를 갖는 인스턴스를 생성한다.

이 메서드들은 this에 바인딩된 name과 같은 프로퍼티 뿐만 아니라 자신이 생성했을 때의 렉시컬 환경에 속한 변수 counter를 기억하고 서로 공유한다.

이 때 생성자 함수 Counter의 변수 counter는 this에 바인딩된 프로퍼티가 아니라 변수이다.
this에 바인딩된 프로퍼티라면 생성한 인스턴스를 통해 외부에서 접근 가능한 `public` 프로퍼티가 되지만 변수 counter는 접근 불가능 하다.
<br/>

하지만 생성자 함수 Counter로 생성한 인스턴스의 메서드인 increase, decrease는 클로저이기 때문에 자신이 생성되었을 때의 렉시컬 환경에 속한 변수인 counter에 접근할 수 있다.
이러한 클로저의 특징을 이용해 클래스 기반 언어의 `private`키워드를 흉내낼 수도 있다.

[참고]

- [Closure | PoiemaWeb](https://poiemaweb.com/js-closure)

<br>
