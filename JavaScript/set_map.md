# Object, Set, Map

Object, Set, Map은 데이터를 저장하고 조작하는데 사용되는 자료구조이다.<br/>
이들의 차이점은 아래와 같다.

### Object

key-value 쌍으로 데이터를 저장한다. key는 문자열 또는 Symbol이며, value는 어떤 타입이든 될 수 있다. key는 유일해야 하며, 순서를 보장하지 않는다.

```js
let object = new Object();

obj.name = "yuna";
obj.age = 29;
obj.hasMoney = false;

console.log(obj); // { name: 'yuna', age: 29, hasMoney: false }
```

### Map

key-value 쌍으로 데이터를 저장한다. key와 value는 모두 어떤 타입이든 될 수 있으며 key의 유일성을 보장한다. Map은 순서가 보장되지 않으며 데이터에 빠른 접근을 제공한다.

```js
let map = new Map();

map.set(1, "str1");
console.log(map.get(1)); // 'str1'
```

### Set

유일한 값을 저장하는 자료구조다. Set은 순서가 없는 중복되지 않은 데이터의 집합이다.

```js
let set = new Set();

set.add(1);
set.add(2);
set.add(3);
set.add(2);

console.log(set.size); // 3
```

**이터레이션**

- `for...of` / `forEach`

```js
// for...of
for (let v of set) console.log(v);

// forEach
set.forEach((v) => console.log(v));
```

<br/>
따라서 이들 자료구조를 선택할 때는 다음과 같은 고려사항이 있다.

- `데이터의 중복 여부`: 데이터 중복을 허용하지 않으면 Set, 그렇지 않으면 Object, Map을 사용하자.
- `순서 보장`: Set은 데이터의 삽입 순서를 보장하지만, 검색이나 순회 시에는 순서를 보장하지 않기 때문에 Object, Map을 별도로 처리하여 순서를 보장한다.
- `데이터에 대한 빠른 접근`: 빠른 접근이 필요한 경우, Object 또는 Map을 사용하자. Set도 물론 빠르지만, Key가 존재하지 않아 검색이나 수정이 어렵다.

<br/>

## Map과 Object 비교하기

Map과 Object는 key-value 쌍으로 데이터를 저장하지만, 몇 가지 차이점이 존재한다.

1.  **키의 타입**
    <br/>Object는 string, symbol 타입의 key만 사용할 수 있다. Map은 객체를 포함한 모든 타입을 key로 사용할 수 있다.
    ```js
    // key로 object 사용
    let yubin = { name: "yubin" };
    let coolGamjaMap = new Map();
    coolGamjaMap.set(yubin, "cool~");

        console.log(coolGamjaMap.get(yubin)); // 'cool~'
        ```

2.  **요소 순서**<br/>
    Object는 추가된 순서와 상관없이 저장되어 반복 시 순서가 일정하지 않다. Map은 삽입 순서를 보장하여 추가된 순서대로 순회할 수 있다.

3.  **크기 확인**<br/>
    Map은 `size` 프로퍼티를 통해 요소의 개수를 확인할 수 있지만 Object는 이러한 메서드나 속성이 존재하지 않는다.
    ```js
    console.log(coolGamjaMap.size); // 1
    ```

4. **상속**<br/>
   Object는 상속 관계를 형성할 수 있으나, Map은 독립된 자료구조로 상속할 수 없다.

5. **이터레이션**<br/>
   Object는 `for...in` 문법을 통해 요소들을 순회할 수 있다. Map은 `for...of`, `forEach` 메서드를 통해 요소들을 순회한다. <br/>
   Object, Map 두가지 자료형 모두 `keys()`, `values()`, `entries()`로 순회할 수도 있다.

6. **성능**<br/>
   Object는 일반적으로 해시 테이블을 사용하며, key-value 쌍의 개수에 따라 성능이 변할 수 있지만 Map은 내부적으로 해시 테이블 또는 이중 연결 리스트로 구현되며, 대부분의 상황에서 일관된 성능을 제공한다.

<br/>
차이점을 테이블로 정리해보았다.

<br/>

| 비교 항목      | Object                           | Map                               |
| -------------- | -------------------------------- | --------------------------------- |
| **키의 타입**  | 문자열 또는 심볼만 가능          | 모든 데이터 타입 가능 (객체 포함) |
| **요소 순서**  | 보장되지 않음                    | 추가된 순서가 보존됨              |
| **크기 확인**  | 별도의 메서드/속성 없음          | `size` 프로퍼티 제공              |
| **상속**       | 프로토타입 체인을 통한 상속 가능 | 독립된 자료구조, 상속 없음        |
| **이터레이션** | `for...in`                       | `for...of`, `forEach`             |
| **성능**       | 키 개수에 따라 성능 변동         | 일관된 성능 제공                  |


이러한 차이점을 고려하여 데이터를 저장하는데 가장 적합한 자료구조를 선택해야 한다.

<br/>
[참고]

- [Map, Set, Object 비교](https://velog.io/@hwisaac/Map-Set-Object-%EB%B9%84%EA%B5%90)
