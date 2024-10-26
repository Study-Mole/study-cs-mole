# Test Code 개념

<br/>
현재 많은 웹 서비스와 함께 UX/UI가 강조되고 프론트앤드 단에서도 많은 비지니스 로직을 다루게 되고 있다.

따라서 프론트앤드 단에서의 테스트도 점차 중요해지고 있는데

다양한 테스트 방법이 있는데 그중 일반적으로 작성되는 테스트 방식은 단위 테스트 (Unit Test) 이다

일반적으로 스냅샷 테스트, e2e 테스트등 다양한 테스트들이 존재한다.

테스트는 기본적으로 무언가를 검증하고 확인하는 용도이기 때문에

어떤것을 확인할것인가! 에 따라 적용하는 테스트 종류가 다르다

---

### 단위 테스트

특정한 모듈 혹은 단위가 기능을 의도한 대로 올바르게 수행하고 있는지 확인하는 것

테스트 중에서도 가장 기본이고 기저에 깔려 있어야 하는 테스트

단위테스트의 경우 범위가 한정정이므로 각 단위에 대한 테스트 코드의 양이 적고 효율적이다. 하지만 명세가 변경되면 가장 영향을 많이 받기 때문에 설계에 주의해야한다.

## ![alt text](image.png)

### 테스트의 목표

1. 정확성 및 신뢰성 확보
2. 수월한 리팩터링

### 무엇을 테스트 하고 싶은지?

- 우리는 비지니스 로직을 테스트해야 한다.

1. 코드가 잘 동작 하는지 확인 (유저의 이벤트 기반 테스트)
2. 버튼을 클릭 했을때 의도한 대로 잘 동작하는지

---

### 효과적인 테스트 코드 작성 방법

테스트 코드는 한번 만들고 끝나는 것이 아닌 계속 유지 보수되고 발전시켜야 한다.

아래는 좋은 테스트 코드를 작성하기 위해 알거나 도움이 되면 좋은 것들이다.

### F.I.R.S.T 원칙

단위 테스트가 가져야 할 특성과 원칙

- Fast: 단위 테스트는 빨라야 한다.
- Isolated: 단위 테스트는 외부 요인에 종속적이지 않고 독립적으로 실행되어야 한다.
- Repeatable: 단위 테스트는 실행할 때마다 같은 결과를 만들어야한다.
- Self-validating: 단위 테스트는 스스로 테스트를 통과했는지 아닌지 판단할 수 있어야 한다.
- Timely/Through
  - Timely: 단위 테스트는 프로덕션 코드가 테스트에 성공하기 전에 구현되어야 한다
  - Through: 단위테스트는 성공적인 흐름 뿐만 아니라 가능한 모든 에러나 비 정상적인 흐름에 대해서도 대응해야 한다.

### 테스트 코드는 DAMP 하게

테스트 코드에서는 DAMP (Descriptive And Meaningful Phrases)하게 작성하라고 이야기한다.

즉 서술적이고 의미있게 작성하라는 이야기다.

일반적인 방법론인 DRY 중복을 줄이는 원칙과 충돌하지만 테스트코드는 중복이 발생해도 직관적이고 명확하게 이해되도록 테스트 코드를 작성하도록 하는게 좋다.

ex) Damp 예시

```jsx
// 첫 번째 테스트 코드
it("설명을 생략하면 이해가 되시나요?", async () => {
  const user = userEvent.setup();
  render(<Component {...preDefinedProps} />);

  const buttonElement = screen.getByRole("button", { name: "버튼입니다" });
  await user.click(buttonElement);

  expect(doSomething).toBeCalledWith(1234);
});

// 두 번째 테스트 코드
it("설명이 없어도 대충 느낌은 오시죠?", async () => {
  const something = 1234;
  const doSomething = vi.fn();
  const user = userEvent.setup();
  render(
    <Component
      {...preDefinedProps}
      something={something}
      doSomething={doSomething}
    />
  );

  const buttonElement = screen.getByRole("button", { name: "버튼입니다" });
  await user.click(buttonElement);

  expect(doSomething).toBeCalledWith(1234);
});
```

### Given - When - Then

테스트 코드는 주어진 상황에 대해 결과를 검증하는게 목적이다.

그래서 이 패턴을 이용하면 좀더 사용자 행위를 기반으로 테스트 시나리오를 정의할 수 있게 한다.

- Given : 테스트를 하기 위해 세팅하는 주어진 환경
- When : 테스트를 하기 위한 조건으로 프론트엔드에선 사용자와의 상호작용인 경우도 많다
- Then : 예상 결과를 나타내며 의도한대로 동작하는지 검증 및 확인 할수 있다.

ex)

```jsx
it("버튼을 1회 클릭하면 1번 클릭했다는 문구가 노출된다", async () => {
  // Given: 사용자와 화면이 준비되어 있고, 화면에는 버튼이 존재함
  const user = userEvent.setup();
  render(<Component />);

  // When: 사용자가 '여기를 눌러보세요'라는 버튼을 클릭함
  const buttonElement = screen.getByRole("button", {
    name: "여기를 눌러보세요",
  });
  await user.click(buttonElement);

  // Then: 문구가 나타나는지 검증함
  expect(screen.getByText("버튼을 1번 클릭했습니다.")).toBeInTheDocument();
});
```

### 개별 테스트 케이스의 목적은 명확히

각 테스트 케이스에 해당하는 테스트 코드를 작성할때 작성하고 있는 테스트 케이스의 목적이 무엇인지 명확히 생각하고 테스트 코드를 작성해야한다.

ex ) 추후 작성

### 테스트 코드의 설명도 중요하다.

테스트 코드의 설명을 대충 쓰거나 필요한 내용을 빠트리는 경우가 있는데 설명이 잘 작성되어있어야 기능을 수정 또는 확장할때 또는 디버깅할때 도움이 많이된다.

설명을 할때는 구현내용이 아닌 함수를 밖에서 본다고 생각하고 만들어야 한다.

구현내용에 집중하면 명세가 아니라 코드의 해석 설명이 나올수도 있다.

ex).

```jsx
// Hmm.. - 내부 구현에 집중되어 있고 명확하지 못한 테스트 코드입니다.
it("입력한 숫자 문자열의 첫자리가 3,4,7,8일 때만 true, 이외엔 false를 반환한다", () => {
  expect(doSomething("1234567")).toBe(false);
  expect(doSomething("2134567")).toBe(false);
  expect(doSomething("3124567")).toBe(true);
  expect(doSomething("4123567")).toBe(true);
  expect(doSomething("5123467")).toBe(false);
  expect(doSomething("6123457")).toBe(false);
  expect(doSomething("7123456")).toBe(true);
  expect(doSomething("8123456")).toBe(true);
});

// Good! - 간결하고 명확하게 함수의 명세와 정책을 담아 설명을 표현합시다.
it("2000년 이후에 태어난 사람의 주민번호 뒷자리인지 검증한다", () => {
  expect(doSomething("1234567")).toBe(false);
  expect(doSomething("2134567")).toBe(false);
  expect(doSomething("3124567")).toBe(true);
  expect(doSomething("4123567")).toBe(true);
  expect(doSomething("5123467")).toBe(false);
  expect(doSomething("6123457")).toBe(false);
  expect(doSomething("7123456")).toBe(true);
  expect(doSomething("8123456")).toBe(true);
});
```

# Jest 사용법 정리

### 프로젝트 생성

새 프로젝트 생성

```jsx
mkdir my-jest-app
cd my-jest-app
npm init -y

```

Jest 설치

```jsx
npm install --save-dev jest
```

설치 완료후 package.json 파일에 테스트 명령어 추가

```jsx
{
  "scripts": {
    "test": "jest"
  }
}

```

### 테스트 코드 예시

test.js 파일 생성후

```
test("1 is 1", () => {
  expect(1).toBe(1);
});
```

이후 터미널에 npm test 를 실행하게 되면 초록색 글씨로 테스트가 통과했다고 나온다.

```bash
$ npm test

> my-jest@1.0.0 test /my-jest
> jest

 PASS  ./test.js
  ✓ 1 is 1 (3ms)

Test Suites: 1 passed, 1 total
Tests:       1 passed, 1 total
Snapshots:   0 total
Time:        0.868s, estimated 1s
Ran all test suites.
```

일반적으로 테스트는 다음과 깉이 일정한 패턴으로 작성한다.

```jsx
test("테스트 설명", () => {
  expect("검증 대상").toXxx("기대 결과");
});
```

toXxx 부분에 사용되는 함수를 Test Matcher라 한다

그리고 npm test를 실행하면 프로젝트 내에 모든 테스트 파일을 찾아서 테스트를 실행해준다. Jest는 기본적으로 test.js 로 끝나고나,

test 디렉터리 안에 있는 파일을 모두 테스트 파일로 인식한다

따라서 특정 테스트 파일만 실행하고 싶은 경우는 npm test<파일명 이나 경로> 를 입력하면 된다.

---

## 3. **비동기 코드 테스트하기 (시간이 걸리는 코드)**

때로는 코드가 **시간이 걸리는 작업**을 할 수 있어요. 예를 들어 서버에서 데이터를 가져오는 코드를 테스트해 볼게요.

### 3.1. 비동기 함수 작성하기

1초 후에 데이터를 돌려주는 함수를 만듭니다. (`fetchData.js`)

```jsx
javascript
코드 복사
// fetchData.js
function fetchData() {
  return new Promise((resolve) => {
    setTimeout(() => resolve('데이터 로드 완료!'), 1000);
  });
}
module.exports = fetchData;

```

### 3.2. 비동기 함수 테스트하기

아래처럼 `fetchData.test.js` 파일을 만들어요.

```jsx
javascript
코드 복사
// fetchData.test.js
const fetchData = require('./fetchData');

test('비동기 데이터는 "데이터 로드 완료!"가 되어야 한다.', async () => {
  const data = await fetchData();  // 데이터를 기다림
  expect(data).toBe('데이터 로드 완료!');  // 결과가 맞는지 확인
});

```

- **`async` / `await`**: 비동기 코드를 기다렸다가 실행하는 방법이에요.

터미널에서 `npm test`를 실행해 보세요! 성공하면:

```bash
bash
코드 복사
PASS  ./fetchData.test.js
✓ 비동기 데이터는 "데이터 로드 완료!"가 되어야 한다. (1001ms)

```

---

## 4. **모킹(Mock) 테스트하기: 가짜 데이터 사용하기**

서버에서 데이터를 받아오는 코드 대신, 가짜 데이터를 써서 테스트할 수 있어요.

### 4.1. 데이터 요청 함수 만들기 (`user.js`)

```jsx
javascript
코드 복사
// user.js
const fetch = require('node-fetch');

async function getUser(id) {
  const response = await fetch(`https://jsonplaceholder.typicode.com/users/${id}`);
  return response.json();
}

module.exports = getUser;

```

### 4.2. 모킹 테스트 코드 작성 (`user.test.js`)

```jsx
javascript
코드 복사
// user.test.js
const getUser = require('./user');
jest.mock('node-fetch', () => jest.fn());  // fetch를 가짜로 만들기

const fetch = require('node-fetch');  // 가짜 fetch 가져오기

test('유저 정보 가져오기', async () => {
  fetch.mockResolvedValue({
    json: () => ({ id: 1, name: '홍길동' }),  // 가짜 데이터
  });

  const user = await getUser(1);  // 유저 가져오기
  expect(user.name).toBe('홍길동');  // 이름이 맞는지 확인
});

```

이제 `npm test`로 테스트하면 가짜 데이터를 사용한 테스트도 성공합니다!

---

**React 컴포넌트 테스트하기**

만약 **React**를 사용하고 있다면, 아래처럼 컴포넌트도 테스트할 수 있어요.

### 6.1. React 컴포넌트 만들기 (`Hello.js`)

```jsx
javascript
코드 복사
// Hello.js
import React from 'react';

function Hello({ name }) {
  return <h1>Hello, {name}!</h1>;
}

export default Hello;

```

### 6.2. 테스트 코드 작성하기 (`Hello.test.js`)

```jsx
javascript
코드 복사
// Hello.test.js
import { render, screen } from '@testing-library/react';
import Hello from './Hello';

test('컴포넌트가 "Hello, 홍길동!"를 렌더링해야 한다.', () => {
  render(<Hello name="홍길동" />);
  expect(screen.getByText('Hello, 홍길동!')).toBeInTheDocument();
});

```

**React 테스트 실행**은 이전과 똑같이 `npm test` 명령어로 할 수 있어요!

---

## Jest Matcher 함수

Jest에서는 Matcher가 아주 중요하다. expect()로 실제 값을 예상하고, Matcher를 사용해 기대한 결과와 일치하는지 비교한다.

### 1. 기본 Matcher : toBe()와 toEqual()의 차이

toBe() - 정확한 일치 비교(===)

```jsx
test("toBe: 정확한 값 비교", () => {
  expect(2 + 2).toBe(4); // 4와 정확히 일치하는지 확인
});
```

- **원시값(숫자, 문자열, 불리언)** 같은 경우 정확히 일치해야 통과

toEqual() - 객체나 배열의 값 비교

```jsx
test("toEqual: 객체 비교", () => {
  const obj = { name: "홍길동" };
  expect(obj).toEqual({ name: "홍길동" }); // 객체 내부 값이 같은지 비교
});
```

- 객체나 배열은 **참조값**이 달라도 내부 내용이 같으면 통과합니다.

### 2. 참/거짓 관련 Matcher

toBeTruthy() - 참(true)처럼 평가되는 값

```jsx
test("toBeTruthy: 참처럼 평가되는 값", () => {
  expect(1).toBeTruthy(); // 1은 true처럼 평가됨
});
```

toBeFalsy() = 거짓(false)처럼 평가되는 값

```jsx
test("toBeFalsy: 거짓처럼 평가되는 값", () => {
  expect(0).toBeFalsy(); // 0은 false처럼 평가됨
});
```

### 3. 숫자 관련 Matcher

toBeGreaterThan() - 크다

```jsx
test("toBeGreaterThan: 5는 3보다 크다", () => {
  expect(5).toBeGreaterThan(3);
});
```

toBeGreaterThanOrEqual() - 크거나 같다

```jsx
test("toBeGreaterThanOrEqual: 5는 5보다 크거나 같다", () => {
  expect(5).toBeGreaterThanOrEqual(5);
});
```

toBeLessThan() - 작다

```jsx
test("toBeLessThan: 3은 5보다 작다", () => {
  expect(3).toBeLessThan(5);
});
```

toBeCloseTo() - 부동소수점 숫자 비교

부동소수점 숫자(소수점 아래 긴 숫자)는 `toBe`로 정확히 비교하기 어렵기 때문에 `toBeCloseTo`를 사용한다.

```jsx
test("toBeCloseTo: 소수점 비교", () => {
  expect(0.1 + 0.2).toBeCloseTo(0.3); // 0.30000000000000004 문제 해결!
});
```

### 4. 문자열 관련 Matcher

toMatch() - 정규표현식으로 문자열 비교

```jsx
test("toMatch: 문자열에 특정 단어 포함", () => {
  expect("Hello Jest").toMatch(/Jest/); // 'Jest'가 포함되어 있는지 확인
});
```

### 5. 배열과 iterable 관련 Matcher

toContain() - 배열에 특정 값 포함

```jsx
test("toContain: 배열에 값 포함", () => {
  expect([1, 2, 3]).toContain(2); // 2가 배열에 포함되어 있는지 확인
});
```

toContainEqual() - 객체 배열에 특정 객체 포함

```jsx
test("toContainEqual: 객체 배열에 같은 객체 포함", () => {
  expect([{ id: 1 }, { id: 2 }]).toContainEqual({ id: 2 });
});
```

### 6. **예외 처리 (에러) 관련 Matcher**

toThrow() - 에러를 던지는지 확인

```jsx
function throwError() {
  throw new Error("에러 발생!");
}

test("toThrow: 에러 발생 테스트", () => {
  expect(() => throwError()).toThrow("에러 발생!");
});
```

### 7. **비동기 코드 관련 Matcher**

`resolves` – **Promise가 성공했는지 확인**

```jsx
test("resolves: 비동기 함수 성공 확인", async () => {
  const promise = Promise.resolve("성공!");
  await expect(promise).resolves.toBe("성공!");
});
```

`rejects` – **Promise가 실패했는지 확인**

```jsx
test("rejects: 비동기 함수 실패 확인", async () => {
  const promise = Promise.reject("실패!");
  await expect(promise).rejects.toBe("실패!");
});
```

---

### 8. **Mock 함수와 호출 관련 Matcher**

`toHaveBeenCalled()` – **함수가 호출되었는지 확인**

```jsx
const mockFn = jest.fn();

test("toHaveBeenCalled: 함수가 호출되었는지 확인", () => {
  mockFn();
  expect(mockFn).toHaveBeenCalled(); // mockFn이 호출된 적 있는지 확인
});
```

`toHaveBeenCalledTimes()` – **함수가 몇 번 호출되었는지 확인**

```jsx
test("toHaveBeenCalledTimes: 정확한 호출 횟수 확인", () => {
  mockFn();
  mockFn();
  expect(mockFn).toHaveBeenCalledTimes(2); // 2번 호출되었는지 확인
});
```

`toHaveBeenCalledWith()` – **특정 인수로 호출되었는지 확인**

```jsx
test("toHaveBeenCalledWith: 특정 인수로 호출 확인", () => {
  mockFn("Hello", 42);
  expect(mockFn).toHaveBeenCalledWith("Hello", 42); // 인수 확인
});
```

---

### 9. **Snapshot 테스트 (React 컴포넌트 등 UI 테스트)**

Jest는 **스냅샷(snapshot)**을 사용해 컴포넌트가 예상대로 렌더링되는지 확인할 수 있다

```jsx
import renderer from "react-test-renderer";
import Hello from "./Hello";

test("Hello 컴포넌트 스냅샷 테스트", () => {
  const tree = renderer.create(<Hello name="홍길동" />).toJSON();
  expect(tree).toMatchSnapshot(); // 스냅샷과 비교
});
```

- 첫 실행 시 스냅샷 파일이 생성된다.
- 코드가 바뀌면 스냅샷과 달라져 테스트가 실패

---

## 10. **마무리: 언제 어떤 Matcher를 써야 할까?**

- **정확한 값 비교**: `toBe()`
- **객체/배열 비교**: `toEqual()`
- **참/거짓 비교**: `toBeTruthy()`, `toBeFalsy()`
- **숫자 비교**: `toBeGreaterThan()`, `toBeLessThan()`
- **문자열 비교**: `toMatch()`
- **배열에 값 포함 확인**: `toContain()`
- **에러 테스트**: `toThrow()`
- **비동기 함수 테스트**: `resolves`, `rejects`

---

## 1. **프로젝트 생성 및 Jest 설치**

### 1.1. 새 프로젝트 생성

```bash

mkdir my-jest-app
cd my-jest-app
npm init -y

```

이 명령어로 프로젝트를 생성한 후, `npm init -y` 명령어로 기본적인 설정 파일(`package.json`)을 만듭니다.

### 1.2. Jest 설치

```bash
npm install --save-dev jest
```

`jest`를 개발 환경에서만 사용하기 위해 `--save-dev` 옵션을 사용합니다.

### 1.3. `package.json` 설정

```json
{
  "scripts": {
    "test": "jest"
  }
}
```

이제 `npm test` 명령어를 통해 모든 테스트 파일이 자동으로 실행됩니다.

---

## 2. **테스트 코드 작성과 실행**

### 2.1. 간단한 테스트 코드 작성 (`test.js`)

```jsx
// test.js
test("1 is 1", () => {
  expect(1).toBe(1);
});
```

이 예제는 간단하게 **1이 1과 일치하는지**를 테스트합니다. 이때 `test()` 함수는 **테스트 단위**를 정의하며, `expect()` 함수는 **결과를 예상**하고, 그 값이 예상과 같은지 확인합니다.

### 2.2. 테스트 실행

```bash
npm test
```

이 명령어를 입력하면 모든 테스트가 실행되고, 테스트 결과가 콘솔에 출력됩니다.

### 2.3. 테스트 결과 예시

```bash

PASS  ./test.js
  ✓ 1 is 1 (3ms)

Test Suites: 1 passed, 1 total
Tests:       1 passed, 1 total
Snapshots:   0 total
Time:        0.868s, estimated 1s

테스트가 성공하면 **초록색 PASS 메시지**가 출력됩니다.
```

---

## 3. **비동기 코드 테스트하기**

비동기 함수는 **Promise 또는 async/await**를 사용하므로 이를 테스트하는 방법을 알아보겠습니다.

### 3.1. 비동기 함수 작성 (`fetchData.js`)

```jsx
// fetchData.js
function fetchData() {
  return new Promise((resolve) => {
    setTimeout(() => resolve("데이터 로드 완료!"), 1000);
  });
}
module.exports = fetchData;
```

이 함수는 **1초 후에 데이터를 반환**하는 비동기 함수입니다.

### 3.2. 비동기 테스트 코드 작성 (`fetchData.test.js`)

```jsx
// fetchData.test.js
const fetchData = require("./fetchData");

test('비동기 데이터는 "데이터 로드 완료!"가 되어야 한다.', async () => {
  const data = await fetchData();
  expect(data).toBe("데이터 로드 완료!");
});
```

- **`async` / `await`**: 비동기 작업이 완료될 때까지 기다립니다.
- 이 코드가 통과되면 **비동기 함수가 예상대로 동작**한다는 의미입니다.

---

## 4. **Mock 테스트: 가짜 데이터로 테스트하기**

실제 서버를 테스트하기엔 부담이 되기 때문에, **Mock 함수로 가짜 데이터를 만들어** 테스트할 수 있습니다.

### 4.1. 데이터 요청 함수 (`user.js`)

```jsx
const fetch = require("node-fetch");

async function getUser(id) {
  const response = await fetch(
    `https://jsonplaceholder.typicode.com/users/${id}`
  );
  return response.json();
}
module.exports = getUser;
```

### 4.2. Mock 테스트 코드 작성 (`user.test.js`)

```jsx
const getUser = require("./user");
jest.mock("node-fetch", () => jest.fn());

const fetch = require("node-fetch");

test("유저 정보 가져오기", async () => {
  fetch.mockResolvedValue({
    json: () => ({ id: 1, name: "홍길동" }),
  });

  const user = await getUser(1);
  expect(user.name).toBe("홍길동");
});
```

- 이 코드는 실제 API 호출 없이 **가짜 데이터를 사용해 테스트**합니다.
- **Mock 함수**를 사용하면 **테스트가 외부 환경에 의존하지 않게** 됩니다.

---

## 5. **React 컴포넌트 테스트**

React 컴포넌트의 렌더링 결과를 테스트해보겠습니다.

### 5.1. React 컴포넌트 작성 (`Hello.js`)

```jsx
import React from "react";

function Hello({ name }) {
  return <h1>Hello, {name}!</h1>;
}

export default Hello;
```

### 5.2. 테스트 코드 작성 (`Hello.test.js`)

```jsx
import { render, screen } from "@testing-library/react";
import Hello from "./Hello";

test('컴포넌트가 "Hello, 홍길동!"를 렌더링해야 한다.', () => {
  render(<Hello name="홍길동" />);
  expect(screen.getByText("Hello, 홍길동!")).toBeInTheDocument();
});
```

- *`@testing-library/react`*를 사용해 컴포넌트를 렌더링하고 DOM을 탐색합니다.

---

## 6. **Jest Matcher 함수 정리**

### 6.1. 기본 Matcher: `toBe()`와 `toEqual()`

- **`toBe()`**: 원시 값(숫자, 문자열)이 정확히 일치하는지 확인합니다.
- **`toEqual()`**: 객체나 배열의 **내부 값**이 같은지 확인합니다.

```jsx
test("toBe: 정확한 값 비교", () => {
  expect(2 + 2).toBe(4);
});

test("toEqual: 객체 비교", () => {
  const obj = { name: "홍길동" };
  expect(obj).toEqual({ name: "홍길동" });
});
```

### 6.2. 참/거짓 Matcher

```jsx
test("toBeTruthy: 참처럼 평가되는 값", () => {
  expect(1).toBeTruthy();
});

test("toBeFalsy: 거짓처럼 평가되는 값", () => {
  expect(0).toBeFalsy();
});
```

### 6.3. 숫자 Matcher

```jsx
test("toBeGreaterThan: 5는 3보다 크다", () => {
  expect(5).toBeGreaterThan(3);
});

test("toBeCloseTo: 소수점 비교", () => {
  expect(0.1 + 0.2).toBeCloseTo(0.3);
});
```

### 6.4. 문자열 Matcher

```jsx
test("toMatch: 문자열에 특정 단어 포함", () => {
  expect("Hello Jest").toMatch(/Jest/);
});
```

### 6.5. 배열 Matcher

```jsx
test("toContain: 배열에 값 포함", () => {
  expect([1, 2, 3]).toContain(2);
});
```

---

## 7. **Snapshot 테스트**

스냅샷 테스트를 사용하면 컴포넌트가 예상대로 렌더링되는지 확인할 수 있습니다.

```jsx
import renderer from "react-test-renderer";
import Hello from "./Hello";

test("Hello 컴포넌트 스냅샷 테스트", () => {
  const tree = renderer.create(<Hello name="홍길동" />).toJSON();
  expect(tree).toMatchSnapshot();
});
```

---

## 8. **마무리: 언제 어떤 Matcher를 사용해야 할까?**

- **정확한 값 비교**: `toBe()`
- **객체/배열 비교**: `toEqual()`
- **참/거짓 평가**: `toBeTruthy()`, `toBeFalsy()`
- **숫자 비교**: `toBeGreaterThan()`, `toBeCloseTo()`
- **문자열 비교**: `toMatch()`
- **배열 값 확인**: `toContain()`
- **에러 발생 테스트**: `toThrow()`
- **비동기 함수 테스트**: `resolves`, `rejects`
