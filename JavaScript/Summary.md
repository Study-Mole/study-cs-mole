# JavaScript Summary

- [호이스팅](#호이스팅)
- [구조 분해 할당 (Destructing assignment)](#구조-분해-할당-destructing-assignment)
- [`const` vs `function`](#const-vs-function)
- [화살표 함수](#화살표-함수)
- [`==` vs `===`](#-vs--equality-operators)
- [클로저](#클로저-closure)
- [`new` 연산자](#new-연산자)
- [배열 처리 함수](#배열-처리-함수)
- [이벤트 버블링](#이벤트-버블링)
- [자바스크립트가 비동기를 처리하는 방법](#자바스크립트가-비동기를-처리하는-방법)


## 호이스팅
호이스팅은 JavaScript 인터프리터가 함수 내 변수 및 함수 선언을 해당 유효 범위의 최상단으로 끌어올려 처리하는 특징을 말합니다. `var`와 `함수 선언문`의 경우, 호이스팅 덕분에 선언 이전에도 변수 참조나 함수 호출이 가능합니다. 하지만 `let`, `const`, 그리고 `함수 표현식`은 호이스팅되더라도 초기화되지 않은 상태로 남아 있어, 선언 전에 접근하면 참조 에러가 발생합니다. 호이스팅은 변수와 함수를 유연하게 사용할 수 있도록 해주지만, 의도치 않은 결과를 초래할 수 있으므로 적절한 키워드 사용이 필요합니다.

## 구조 분해 할당 (Destructing assignment)

구조 분해 할당이란 배열 또는 객체의 속성을 해체하여 그 값을 개별 변수에 담을 수 있게 하는 Javascript 표현식 입니다. 이를 통해 기본값 할당, 변수 교환, 객체나 배열의 요소 추출, 함수 매개변수 작업 등 복잡한 데이터 구조를 더 간결하게 처리할 수 있습니다.

## `const` vs `function`

`const`와 `function`은 JavaScript에서 함수를 정의할 때 사용하는 키워드입니다. `const`로 선언한 변수에 익명 함수를 할당하는 방식, `function` 키워드를 사용해서 함수를 선언하는 방식으로 사용됩니다. `const`는 `function`과 달리 호이스팅이 지원되지 않아 함수 호출이 선언보다 먼저일 수 없고, 이미 선언된 변수에 다른 함수를 재할당 할 수는 없습니다. 하지만 코드를 깔끔하게 작성할 수 있어 유지보수 측면에서 유리하다는 장점을 갖습니다.

## 화살표 함수

화살표 함수는 ES6에서 도입된 간결한 함수 선언 방식으로, `function` 키워드 대신 `=>`를 사용하여 작성됩니다. 일반 함수와 다르게 메서드, 생성자 함수, `addEventListner` 콜백에서 `this`가 객체를 참조하지 않고 상위 스코프를 참조하므로 이런 상황에서는 일반 함수를 사용하는 것이 더 적합합니다.

## `==` vs `===`

`==`과 `===`는 동등 비교를 위해 사용되는 연산자이다. `===`은 값과 타입이 모두 일치하는지 엄격하게 비교한다. 반면에 `==`은 강제 형변환 후에 비교 연산을 수행하기 때문에 비교적 느슨하다. `==`은 형변환으로 인해 의도치 않은 상황이 발생할 수 있기 때문에 개발자가 원하는 동등 비교 시에는 `===`를 사용하는 것이 좋다.

## 클로저 (Closure)

클로저는 함수가 자신이 선언된 환경을 기억하여, 그 환경 외부에서 호출되더라도 그 환경에 접근할 수 있도록 해줍니다. 상태를 유지할 수 있어, 전역 변수를 사용하는 것 대신 특정 상태를 안전하게 유지하고 접근할 수 있습니다. 또한 클래스 기반 언어의 `private` 키워드처럼 외부에 노출되지 않고 내부에서만 접근할 수 있는 상태 또는 메서드를 관리할 수 있습니다.

## `new` 연산자

JavaScript의 `new`는 사용자 정의 객체 타입 또는 객체 타입의 인스턴스를 생성하는 연산자 입니다. `new`는 해당 함수의 `prototype`을 상속받는 새로운 객체를 생성합니다. `new` 연산자를 통해 다양한 객체를 생성하고 `prototype`으로 속성들을 제어하여 객체 지향 프로그래밍을 구현할 수 있습니다.

## 배열 처리 함수

배열 처리 함수에는 `map()`, `filter()`, `reduce()`가 있고 이들은 JavaScript에서 배열을 변형하거나 처리할 때 사용됩니다. 원본 배열 정보를 기반으로 배열의 요소를 다른 형태로 변환, 조건에 받는 요소 추출, 유의미한 값 추출과 같은 로직을 구현할 수 있습니다. 원본 배열을 변경하지 않으면서 데이터를 변환할 수 있다는 것은 이들의 가장 큰 특징입니다.

## 이벤트 버블링

HTML의 계층적 구조로 인해, 특정 요소에 발생한 이벤트가 전달되는 현상을 이벤트 전파라고 하며, 이 때 자식 태그에서 부모 태그로 전파되는 경우를 이벤트 버블링이라고 합니다. 반대 개념인 이벤트 캡처링은 부모에서 자식 태그로 전파되는 경우를 말합니다.

## 자바스크립트가 비동기를 처리하는 방법

Single Thread 기반의 JavaScript는 비동기 작업을 통해 자원 낭비와 사용성 저하를 방지합니다. main thread에서 실행되는 모든 함수는 call stack에 쌓이며, 이곳에 비동기 함수가 들어오면 즉시 백그라운드로 전달됩니다. 우리는 비동기 함수를 호출할 때, 비동기 작업이 완료되는 시점에 main thread에서 실행할 callback 함수를 함께 전달합니다. 백그라운드에서 비동기 작업이 완료되면, 이 callback 함수가 Event Queue에 쌓이게 되고 call stack이 비는 경우 Event Loop가 Event Queue의 callback 함수를 call stack으로 옮기는 역할을 수행합니다.

