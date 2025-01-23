## 1. 기본 타입(Basic Types)

TypeScript의 기본 타입은 JavaScript가 지원하는 원시(primitive) 타입을 대부분 포함한다. 또한 `strictNullChecks` 옵션 여부에 따라 `null`과 `undefined`의 처리 방식이 달라지므로, 이 부분도 유의해야 한다.

### 1.1 `string`

- **설명**: 문자열 타입을 나타낸다.

### 1.2 `number`

- **설명**: 숫자 타입을 나타낸다. JavaScript의 숫자는 모두 부동소수점 64비트 형식이므로 정수/실수를 구분하지 않는다.

### 1.3 `boolean`

- **설명**: 참(`true`) 또는 거짓(`false`)을 나타낸다.

### 1.4 `null` & `undefined`

- **설명**:
  - `null`: 값이 없음을 명시적으로 나타낼 때 사용.
  - `undefined`: 변수가 선언은 되었지만, 값이 할당되지 않았음을 나타낼 때 사용.
- **주의**:
  - `strictNullChecks`가 `true`인 경우, `null`과 `undefined`는 다른 타입에 **자동으로** 할당할 수 없다.
    ```ts
    // strictNullChecks = true
    let str: string = "Hello";
    str = null; // 오류
    str = undefined; // 오류
    ```
  - `strictNullChecks`가 `false`인 경우, `null`과 `undefined`는 모든 타입에 할당 가능(기존 JavaScript 관습 유지).

### 1.5 `symbol`

- **설명**: **고유**하고 **변경 불가능**한 값(ES6+). 주로 객체 프로퍼티의 **유일한 식별자**(key)로 사용한다.
- **예시**:
  ```ts
  const sym1 = Symbol("description");
  const sym2 = Symbol("description");
  console.log(sym1 === sym2); // false
  ```

### 1.6 `bigint`

- **설명**: ES2020에서 추가된 큰 정수 타입으로 2^53 - 1을 초과하는 정수를 안전하게 표현할 수 있다.
- **사용 조건**:
  - `target`이 `ES2020` 이상이어야 하며, `lib` 옵션에 `"ES2020.BigInt"` 등이 포함되어야 한다.
- **예시**:
  ```ts
  let bigNumber: bigint = 12345678901234567890n;
  let anotherBig: bigint = BigInt("12345678901234567890");
  ```

### 1.7 `object`

- **설명**: 원시 타입이 아닌 모든 타입을 의미한다. 그러나 `object` 타입은 실제로는 구체적인 형태를 보장하지 않으므로 활용도가 그리 높지 않다.
- **예시**:
  ```ts
  let obj: object = { name: "Alice" };
  obj = [1, 2, 3]; // 배열도 가능
  // obj.name;      // 오류: 'object' 타입에는 'name' 속성이 없음
  ```

## 2. `any`, `unknown`, `never`

### 2.1 `any`

- **설명**: 어떤 타입이든 허용한다. TypeScript의 **타입 검증**을 사실상 우회하기 때문에, 가능한 한 사용을 지양하는 것이 좋다.
- **예시**:
  ```ts
  let anything: any = "hello";
  anything = 42;
  anything = true;
  // 모든 동작을 허용하지만, 타입 안정성은 포기
  ```

### 2.2 `unknown`

- **설명**:
  - TypeScript 3.0부터 추가된 타입.
  - 알 수 없는 타입이지만, **사용하기 전에 구체적인 타입 좁히기(narrowing)가 필요**하다.
  - `any`와 달리, 그냥 쓸 수 없고 안전하게 쓰기 위해서는 **타입 체크**를 강제한다.
- **예시**:

  ```ts
  let notSure: unknown = 10;
  notSure = "hi";

  // 불가능한 예시 (오류 발생)
  // let str: string = notSure;  // 타입 오류: 'unknown'을 'string'에 바로 할당할 수 없음

  // 타입 좁히기 예시
  if (typeof notSure === "string") {
    // 여기 블록 안에서는 notSure가 string으로 좁혀짐
    let str: string = notSure;
    console.log(str.toUpperCase()); // 정상
  }
  ```

> **정리**: `unknown`은 (a) 런타임에 어떤 타입이 될지 모를 때, (b) 그러나 그 값을 사용할 때는 타입 안정성을 유지해야 할 때 사용한다.

### 2.3 `never`

- **설명**: 결코 발생하지 않는 타입을 의미한다.
  - 예: 함수가 **절대 종료되지 않는 경우**(무한 루프, 예외만 던지는 함수), 혹은 특정 유니언 타입에서 **절대 도달할 수 없는 케이스** 등.
- **예시 1**: 항상 오류를 던지는 함수
  ```ts
  function throwError(message: string): never {
    throw new Error(message);
  }
  ```
- **예시 2**: `switch` 문에서 모든 케이스를 처리했는지 확인할 때 사용

  ```ts
  type Shape =
    | { kind: "circle"; radius: number }
    | { kind: "square"; side: number };

  function getArea(shape: Shape): number {
    switch (shape.kind) {
      case "circle":
        return Math.PI * shape.radius * shape.radius;
      case "square":
        return shape.side * shape.side;
      default:
        // 이 지점에 오면 shape의 kind가 'circle' 또는 'square'가 아닌 경우
        // shape는 절대 발생할 수 없으므로, 여기는 never가 되어야 함
        const _exhaustiveCheck: never = shape;
        return _exhaustiveCheck;
    }
  }
  ```

  > **요약**: `never` 타입은 **정상적으로 값을 생성할 수 없는 경우**에 사용된다.

## 3. 유니언(Union)과 인터섹션(Intersection)

### 3.1 유니언(Union) 타입

- **설명**: 여러 타입 중 **하나**가 될 수 있다는 것을 의미한다.
- **문법**: `type A = string | number;`
- **예시**:

  ```ts
  function printId(id: number | string) {
    console.log("Your ID is " + id);
  }

  let userId: string | number;
  userId = 101; // OK
  userId = "101"; // OK
  // userId = true; // 오류
  ```

### 3.2 인터섹션(Intersection) 타입

- **설명**: 여러 타입을 **모두 만족**해야 함을 의미한다.
- **문법**: `type B = { name: string } & { age: number };`
- **예시**:

  ```ts
  type Person = { name: string } & { age: number };

  // Person 타입은 { name: string, age: number } 형태와 동일
  let p: Person = {
    name: "Alice",
    age: 25,
  };
  ```

> **중요**: 유니언은 “A 혹은 B”, 인터섹션은 “A이면서 B”라는 의미이다.

## 4. 타입 추론(Type Inference)

TypeScript는 변수에 값을 할당하거나, 함수의 반환값을 통해 **자동으로** 타입을 추론하려고 시도한다. 이렇게 추론된 타입을 보고, 굳이 명시적 타입 선언을 하지 않아도 되는 경우가 많다.

### 4.1 변수에서의 타입 추론

```ts
let x = "hello";
// TypeScript는 자동으로 x를 string으로 추론한다.

x = "bye"; // 정상
// x = 42;  // 오류: string 타입에 number는 할당 불가
```

### 4.2 함수 반환값 타입 추론

```ts
function add(a: number, b: number) {
  return a + b;
  // 반환 타입이 number로 추론됨
}

const result = add(1, 2); // result는 number로 추론됨
```

### 4.3 최적의 사용 방법

- **명확한 타입**을 제공할 필요가 없을 때는 **추론에 맡겨** 불필요한 타입 선언을 줄이자.
- 다만, **함수의 매개변수**나 **공개 API**(라이브러리, 모듈 등)는 명시적으로 타입을 작성하는 것이 권장된다(코드 가독성과 안정성).
