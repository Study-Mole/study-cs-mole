# 구조 분해 할당 (Destructing assignment)

배열이나 객체의 속성을 해체하여 그 값을 개별 변수에 담을 수 있게 하는 JavaScript 표현식이다.

## 맛보기

코드로 대강 파악해보자.

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

## 기본 변수 할당

느낌이 좀 오는지?
<br>먼저 배열 분해를 살펴보자.

```javascript
let x = [1, 2, 3, 4, 5];
let [y, z] = x;
console.log(y); // 1
console.log(z); // 2
```

## 선언에서 분리한 할당

변수 선언이 분리되어도 구조 분해로 값을 할당할 수 있다.

```javascript
let a, b;
[a, b] = [1, 2];
console.log(a); // 1
console.log(b); // 2
```

## 기본값

분해한 값이 `undefined` 라면?
<br>기본값이 없으면 그대로 `undefined`를, 있다면 그 기본값을 대신 사용한다.

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

## 변수 교환

하나의 구조 분해 표현식으로 두 변수의 값을 교환할 수 있다.
<br>구조 분해 할당 개념을 찾아보게 된 계기이기도 한! 증말 간단하다.

```javascript
let a = 1;
let b = 2;
[a, b] = [b, a];

console.log(a); // 2
console.log(b); // 1
```

## 함수가 반환한 배열 분석

함수도 구조 분해 할당이 가능하다.

```javascript
function f() {
  return [1, 2];
}

var a, b;
[a, b] = f();
console.log(a); // 1
console.log(b); // 2
```

## 일부 반환값 무시

필요없는 값은 건너뛸 수 있다.

```javascript
function f() {
  return [1, 2, 3];
}

var [a, , b] = f();
console.log(a); // 1
console.log(b); // 3
```

싸그리 무시할 수도 있다.
<br>근데 이런 건 어디에 써먹을라나

```javascript
var [, ,] = f();
```

## 나머지 할당하기

[맛보기](#맛보기)의 rest와 같다.
<br>나머지 요소는 항상 선언된 변수 중 가장 뒤에 위치해야 한다.

```javascript
var [a, ...b] = [1, 2, 3, 4];
console.log(a); // 1
console.log(b); // [2, 3, 4]

var [...c, d] // ❗SyntaxError : 나머지 요소 뒤에 쉼표는 금물
```

## 정규 표현식과 함께 활용해보기

정규 표현식의 exec() 메서드는 일치하는 부분을 찾으면
<br>그 문자열에서 정규식과 일치하는 부분 전체를 배열[0]에,
<br>그 뒤에 정규식에서 ()로 묶인 각 그룹과 일치하는 부분을 포함하는 배열을 반환한다.
<br>입맛대로 잘 쪼갠 뒤 구조 분해 할당으로 원하는 부분만 챙겨올 수 있을 것이다.

```javascript
function letsParse(url) {
  let parsedURL = /^(\w+)\:\/\/([^\/]+)\/(.*)$/.exec(url);
  if (!parsedURL) return false;
  console.log(parsedURL); // ["https://github.com/Study-Mole/study-cs-mole", "https", "github.com", "Study-Mole/study-cs-mole"]

  let [, protocol, fullhost, fullpath] = parsedURL;
  return protocol;
}

console.log(letsParse("https://github.com/Study-Mole/study-cs-mole")); // "https"
```

## 기본 할당

객체 구조 분해이다.

```javascript
let o = { p: 42, q: true };
let { p, q } = o;
console.log(p); // 42
console.log(q); // true
```

## 선언 없는 할당

배열에서 선언 없는 할당이 가능한 것과 같은 맥락이다.
<br>배열 구조 분해와 다르게 할당문을 둘러싼 `(...)`가 필요하다.
<br>이게 없으면 좌변의 { a, b }를 객체 리터럴이 아닌 블록으로 인식해 독립적인 구문이 될 수 없게 된다.

```javascript
let a, b;
({ a, b } = { a: 1, b: 2 });
```

## 기본값

객체로부터 해체된 값이 `undefined`인 경우, 기본값을 할당할 수 있다.

```javascript
let { a = 10, b = 5 } = { a: 3 }; // b는 할당 못받음 ㅠ

console.log(a); // 3
console.log(b); // 5 기본값 줬질옹
```

## 새로운 변수 이름으로 할당하기

속성을 해체하여 원래 속성명을 다른 이름의 변수에 할당할 수 있다.

```javascript
var yubin = { a: 23, c: true };
var { a: age, c: gamja } = yubin;

console.log(age); // 24
console.log(gamja); // true
```

## 중첩된 객체/배열의 구조 분해

살짝 복잡하나, 주석을 잘 따라가보자.

```javascript
let metadata = {
  title: "StudyMole",
  details: [
    {
      locale: "Moles",
      localization_tags: [],
      last_edit: "2024-08-29T14:29:37",
      url: "/Moles/StudyMole",
      title: "Destructing-assignment",
    },
  ],
  url: "/Moles/StudyMole",
};

let {
  title: studyTitle, // metadata의 title을 새로운 변수 studyTitle에 할당
  details: [{ title: topic }], // metadata의 details의 구조를 맞춰주고 내부의 title을 새로운 변수 topic에 할당
} = metadata;

console.log(studyTitle); // "StudyMole"
console.log(topic); // "Destructing-assignment"
```

## for of 반복문과 구조 분해

좀 더 길지만 비교적 간단하다.

```javascript
let moles = [
  {
    name: "yubin",
    age: 23,
    friends: {
      f1: "jomboo",
      f2: "yuna",
      f3: "zhoo",
      f4: "jiyeon",
    },
  },
  {
    name: "yuna",
    age: 27,
    friends: {
      f1: "jomboo",
      f2: "zhoo",
      f3: "yubin",
      f4: "hyehwa",
    },
  },
];

for (let {
  name: n,
  friends: { f4: f },
  age: a,
} of moles) {
  console.log("Name: " + n + ", Age: " + a + ", Friend: " + f);
}

// Name: yubin, Age: 23, Friend: jiyeon
// Name: yuna, Age: 27, Friend: hyehwa
```

## 객체 구조 분해에서 rest

복잡한 건 여까지. 객체 구조 분해에서도 rest 는 같은 방식으로 동작한다.

```javascript
let { a, b, ...rest } = { a: 10, b: 20, c: 30, d: 40 };
console.log(a); // 10
console.log(b); // 20
console.log(rest); // { c: 30, d: 40 }
```

## etc.

평소에 자주 사용하는 아래 예시들도 모두 구조 분해 할당에 해당한다.

### 1. 함수의 매개변수 처리

```javascript
function displayUser({ name, age }) {
  console.log(`Name: ${name}, Age: ${age}`);
}
```

### 2. 객체 속성의 빠른 접근

```javascript
let { username, email } = user;
```

### 3. 배열 요소의 추출

```javascript
let [first, second] = [10, 20];
```

### 4. API 응답 처리

```javascript
let { data, status } = apiResponse;
```

<br>구조 분해 할당은 자바스크립트에서 더 효율적이고 간결하게 코드를 작성하도록 돕는다. 다양한 상황에서 적절히 사용하여 생산성을 높일 수 있다.
