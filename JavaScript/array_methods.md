# 배열 처리 함수

## 함수형 프로그래밍

함수형 프로그래밍은 함수의 출력 값이 오직 함수로 전달된 인자에만 의존하는 프로그래밍 패러다임으로, 동일한 인자를 사용해 함수를 호출하면 항상 같은 결과를 반환한다.
<br>이를 **참조 투명성**이라고 하며, 함수형 프로그래밍의 핵심 개념 중 하나이다.

배열 처리 함수 `map()`, `filter()`, `reduce()`는 배열을 변형하거나 처리할 때, 상태를 변경하지 않고 입력된 배열의 복사본을 기반으로 동작하며, 원본 배열을 변경하지 않기 때문에 `순수 함수`의 개념에 부합하다.
<br>이를 통해 함수 실행마다 예측 가능한 결과를 얻을 수 있다.

전통적인 명령형 프로그래밍에서는 전역 변수나 로컬 변수를 사용해 상태를 변경하는데, 이는 side-effect를 발생시킬 가능성을 높인다.
<br>함수 실행 결과가 매번 다르면 코드의 동작을 이해하고 디버깅하기 어려워진다.
<br>배열 처리 함수들은 이러한 부수 효과를 제거하면서, 복잡한 로직을 간결하게 구현할 수 있도록 도와준다.

## `map()`

클라이언트는 서버로부터 받은 데이터를 변환해 사용하는 경우가 많다.
<br>`map`은 배열의 각 요소를 다른 값으로 변환하여 새로운 배열을 생성하며, 주로 배열의 요소를 다른 형태로 변환할 때 사용된다.

### 사용법

`map`은 인자로 콜백을 받는다. map이 호출될 때, 이 콜백에 현재 **값의 iteration, iteration의 index** 그리고 **원본 배열**이 주어진다.
<br>map을 위한 optional한 두 번째 인자도 있다. 이는 콜백 내부에서 **this**를 이용하기 위한 값이다.

```javascript
const nums = [1, 2, 3];
const doubled = nums.map((num, index, array) => {
  return num * 2;
});

console.log(doubled); // [2, 4, 6]
```

### 예제

아래 자바스크립트 오브젝트 배열이 있다고 하자.

```javascript
const Moles = [
  { id: 1, name: "jomboo", position: "first" },
  { id: 2, name: "yuna", position: "second" },
  { id: 3, name: "zhoo", position: "third" },
  { id: 4, name: "yubin", position: "na" },
];
```

name 값만 모아보자.

```javascript
const allMoleNames = Moles.map((Mole) => {
  return Mole.name;
});

console.log(allMoleNames); // ["jomboo", "yuna", "zhoo", "yubin"];
```

유틸 함수도 껴볼 수 있다.

```javascript
const upperCaseMoles = Moles.map(myMoleFunc);

const myMoleFunc = (Mole) => {
  return Mole.name.toUpperCase();
};

console.log(upperCaseMoles); // ["JOMBOO","YUNA","ZHOO","YUBIN"];
```

프로퍼티를 삭제하거나 추가해보자.

```javascript
const mapped = Moles.map((Mole) => {
  const { position, ...rest } = Mole;

  return {
    ...rest,
    ThisMoleIs: "CoolAndWarm",
  };
});

console.log(mapped);
// [
//   { id: 1, name: "jomboo", ThisMoleIs: "CoolAndWarm" },
//   { id: 2, name: "yuna", ThisMoleIs: "CoolAndWarm" },
//   { id: 3, name: "zhoo", ThisMoleIs: "CoolAndWarm" },
//   { id: 4, name: "yubin", ThisMoleIs: "CoolAndWarm" },
// ];
```

## `filter()`

배열의 각 요소에 대해 조건을 평가한 후, 조건에 맞는 요소들만을 포함하는 새로운 배열을 생성한다.

### 사용법

`filter`는 map과 동일한 인자를 받으며, 비슷하게 동작한다.
<br>유일한 차이점은 콜백 함수가 `true` 또는 `false`를 반환하는 것이다.
<br>`true`를 반환하면 해당 원소가 배열에 남고, `false`이면 필터링되어 제거된다.

```javascript
const nums = [1, 2, 3, 4];
const even = nums.filter((num, index, array) => {
  return num % 2 === 0;
});

console.log(even); // [2, 4]
```

### 예제

간단한 문자열 검색허기

```javascript
const strings = ["hello", "Matt", "Mastodon", "Cat", "Dog"];

const filtered = strings.filter((str) => {
  return str.includes("at");
});

console.log(filtered); // ["Matt", "Cat"];
```

`Moles`에서 나를 찾아보자.

```javascript
const naMole = Moles.filter((Mole) => {
  return Mole.position.toUpperCase() === "NA";
});

console.log(naMole);
```

## `reduce()`

배열 하나를 받아서 하나의 값으로 바꿔준다.

### 사용법

`map`, `filter`와 비슷하게 동작한다. 콜백 인자로 **accumulator**를 받는다는 점에서 차이가 있다.
<br>모든 반환 값을 누적하는 역할으로, `reduce` 함수의 두 번째 인자 값이 accumulator 초기값이다.

```javascript
const nums = [1, 2, 3, 4];
const sum = nums.reduce((acc, curr) => {
  return acc + curr;
}, 0);
const avg = sum / nums.length;

console.log(sum); // 10
console.log(avg); // 2.5
```

### 예제

새로운 `Moles` 자바스크립트 오브젝트 배열을 가져왔다.

```javascript
const Moles = [
  { id: 1, name: "jomboo", team: "fa" },
  { id: 2, name: "yuna", team: "bi" },
  { id: 3, name: "zhoo", team: "tb" },
  { id: 4, name: "yubin", team: "fa" },
];
```

..어디 들어들있는지 알아보자.

```javascript
const obj = Moles.reduce((acc, curr) => {
  const team = curr.team;
  const teamCount = acc[team] ? acc[team] + 1 : 1;

  return {
    ...acc,
    [team]: teamCount,
  };
}, {});

console.log(obj); // { "fa": 2, "bi": 1, "tb": 1};
```

새로운 두더지가 들어왔다.

```javascript
const newMoles = [Moles, [{ id: 5, name: "hanbin", team: "banggoo" }]];

const flatnewMoles = newMoles.reduce((acc, curr) => {
  return acc.concat(curr);
}, []);

console.log(flatnewMoles);
// [
//   { id: 1, name: "jomboo", team: "fa" },
//   { id: 2, name: "yuna", team: "bi" },
//   { id: 3, name: "zhoo", team: "tb" },
//   { id: 4, name: "yubin", team: "fa" },
//   { id: 5, name: "hanbin", team: "banggoo" },
// ]
```

## `map(), filter(), reduce()`

위의 세 함수를 연계하여 사용할 수 있다.

### 사용법

2의 배수만 필터링하고, 각 요소를 제곱하여, 총합을 구해보자.

```javascript
const numbers = [1, 2, 3, 4, 5];

const result = numbers
  .filter((num) => num % 2 === 0) // 2의 배수 필터링
  .map((num) => num ** 2) // 각 요소 제곱
  .reduce((acc, curr) => acc + curr, 0); // 총합 계산

console.log(result); // 20
```

### 예제

조금 더 복잡한 배열을 가져왔다. 각각의 노래는 초로 표기된 길이를 갖는다.

```javascript
const day6Songs = [
  { id: 1, name: "nothing but", artist: "Young K", duration: 208 },
  { id: 2, name: "Unpainted Canvas", artist: "WONPIL", duration: 229 },
  { id: 3, name: "How to love", artist: "Day6", duration: 204 },
  {
    id: 4,
    name: "Right Through Me",
    artist: "Day6 (Even of Day)",
    duration: 218,
  },
];

const btobSongs = [
  { id: 1, name: "SWIMMING", artist: "임현식", duration: 261 },
  { id: 2, name: "AT THE END", artist: "이창섭", duration: 230 },
  { id: 3, name: "Dear My Dear", artist: "서은광", duration: 269 },
  { id: 4, name: "BE SOMEBODY", artist: "육성재", duration: 183 },
];
```

200초가 넘는 모든 노래의 제목을 컴마로 구분하는 배열을 가져와야 한다면?

```javascript
const allSongs = [day6Songs, btobSongs];

const songNames = allSongs
  // 배열 평탄화. const allSongs = [...day6Songs, ...btobSongs];와 같다.
  .reduce((acc, curr) => {
    return acc.concat(curr);
  }, [])
  // 200초 이상 노래 필터링
  .filter((song) => song.duration > 200)
  // 노래 이름만 추출
  .map((song) => song.name)
  // 컴마로 구분된 문자열 생성
  .join(", ");

console.log(songNames); // nothing but, Unpainted Canvas, How to love, Right Through Me, SWIMMING, AT THE END, Dear My Dear
```

### 연계의 이유

`map`, `filter`, `reduce`를 함께 사용하면 `array[i]`와 같은 형식보다 현재 값에 직접 접근하기 쉬워진다.
<br>기존 배열의 변화를 방지하여 side-effect를 최소화할 수 있으며, `for 문`을 관리할 필요가 없어지고 빈 배열을 생성해 push할 필요도 없어진다.

### 결론

배열 처리 함수는 원본 배열을 변경하지 않으면서 데이터를 간결하고 직관적으로 변형하고 집계할 수 있는 강력한 도구이다.
<br>함수형 프로그래밍의 원칙을 잘 반영하여 side-effect를 최소화하고 코드의 가독성을 향상시키는 데 강점을 갖는다.

[참고]

- [Learn map, filter and reduce in Javascript](https://medium.com/@joomiguelcunha/learn-map-filter-and-reduce-in-javascript-ea59009593c4)
