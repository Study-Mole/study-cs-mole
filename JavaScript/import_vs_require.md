# import와 require의 차이점

`import`와 `require`은 JavaScript 모듈을 불러오는데 사용되는 키워드이다. import는 ES6 모듈 시스템의 구문이며, require은 CommonJS 모듈 시스템의 구문이다.

### 1. 구문 및 사용법의 차이
- `import`: ES6 모듈 시스템의 키워드이다. from과 함께 사용되며, 필요한 모듈을 가져온다.
```js
import { module } from 'module';
```
- `require`: CommonJS 모듈 시스템에서 사용되는 함수이다. 필요한 모듈을 가져온다.
```js
const module = require('module');
```

### 2. 호출 시점의 차이
- `import`: **`import` 구문은 모듈을 미리 로드하고 정적으로 평가**된다. 모듈은 스크립트의 최상위 수준에서만 import 할 수 있으며, 조건문이나 함수 내부에서는 동작하지 않는다.
- `require`: **`require` 함수는 동적으로 모듈을 로드**한다. 조건문이나 함수 내부에서도 사용할 수 있으며, 필요한 시점에 모듈을 동적으로 로드할 수 있다.

### 3. 모듈 시스템 지원 여부
- `import`: `import` 구문은 최신 버전의 브라우저와 일부 환경에서 지원된다.
- `require`: `require` 함수는 CommonJS 모듈 시스템의 표준이다. Node.js 환경에서는 기본적으로 지원되며, 브라우저에서는 별도의 번들러나 변환 도구를 사용하여 CommonJS 모듈을 지원해야 한다. 


## CommonJS? ES6?
### CommonJS 모듈
JavaScript를 위한 모듈 표준 중 하나로, 주로 서버 사이드 개발에 사용된다. Node.js는 모듈화를 위해 CommonJS를 사용하고 있다.

CommonJS 모듈은 동기적으로 로드되어, 모듈 로딩 시 모든 모듈 코드를 읽어야 한다. 이는 서버 환경에서는 문제가 되지 않지만, 브라우저 환경에서는 성능 문제를 일으킬 수 있다.
```js
// lib.js
function sum(a, b){
    return a + b;
}
module.exports = sum;
```
```js
// app.js
const sum = require('./lib');
console.log(sum(3, 5));
```
=> lib.js 파일은 sum 함수를 모듈로 내보내고, app.js는 require 함수를 사용하여 해당 모듈을 로드한다.

### ES6 모듈
ES6에서 도입된 모듈 시스템으로, import와 export 문을 사용하여 모듈을 로드하고 정의한다. ES6 모듈은 비동기적으로 로드될 수 있으며, 브라우저와 서버 양쪽에서 사용할 수 있다.

ES6 모듈은 정적 구조를 가지고있어, 컴파일 타임에 모듈 구조가 결정된다. 이는 트리 쉐이킹 같은 최적화 기법을 가능하게 하며, 로딩 시간과 성능을 개선할 수 있다.
```js
// lib.js
export function sum(a, b) {
    return a + b;
}
```
```js
// app.js
import { sum } from './lib';
console.log(sum(2, 3));
```
=> lib.js 파일은 sum 함수를 내보내고, app.js 파일은 import 문을 사용하여 해당 함수를 로드한다. ES6 모듈은 모듈의 의존성을 보다 명확하게 표현할 수 있으며, 코드의 가독성과 유지보수성을 향상시킨다.

<br/>

[참고]
- [자바스크립트의 모듈 시스템: CommonJS와 ES6 모듈의 비교](https://f-lab.kr/insight/javascript-module-system-comparison)
- [import와 require의 차이점이 무엇인가요?](https://velog.io/@dev_seongjoo/import%EC%99%80-require%EC%9D%98-%EC%B0%A8%EC%9D%B4%EC%A0%90%EC%9D%B4-%EB%AC%B4%EC%97%87%EC%9D%B8%EA%B0%80%EC%9A%94)