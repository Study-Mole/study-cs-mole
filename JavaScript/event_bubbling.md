## 이벤트 버블링

HTML 문서의 각 요소들은 태그 안에 태그가 위치하는 **계층적** 형식을 보인다.<br/>
이러한 계층적 구조 때문에 특정 요소에 이벤트가 발생할 경우 연쇄적 이벤트 흐름이 일어나게 된다.

이러한 현상을 이벤트 전파(Event Propagation)라 하며, 전파 방향에 따라 두 가지로 구분된다.

1. 버블링(Bubbling): 자식 태그에서 발생한 이벤트가 부모 태그로 전파
2. 캡처링(Capturing): 자식 태그에서 발생한 이벤트가 부모 태그부터 시작하여 안쪽 자식 태그로 전파

| !Event Bubbling(./images/event-bubble.png) | !Event Capturing(./images/event-capture.png) |
| -------------------------------------------- | ---------------------------------------------- |

### 이벤트 버블링

버블링이란 한 태그에 이벤트가 발생하면 이 태그에 할당된 핸들러가 동작하고, 이어서 부모 태그의 핸들러가 동작하고 최상단의 태그를 만날 때까지 반복되면서 핸들러가 동작하는 현상을 말한다. <br/>
이벤트가 제일 깊은 속에 있는 태그에서 시작해 부모 태그를 거슬러 올라가며 발생하는 모양이 마치 거품과 닮아있다.

```HTML
<body>
  <div class="DIV1">DIV 1
    <div class="DIV2">DIV 2
      <div class="DIV3">DIV 3</div>
    </div>
  </div>
</body>
```

```javascript
const divs = document.querySelectorAll("div");

const clickEvent = (e) => {
  console.log(e.currentTarget.className);
};

divs.forEach((div) => {
  div.addEventListener("click", clickEvent);
});
```

JavaScript는 기본적으로 버블링이 발생하기 때문에 `DIV3`을 클릭한다면 `DIV3` -> `DIV2` -> `DIV1`의 순서대로 이벤트가 전파된다.

> 거의 모든 이벤트는 버블링이 된다.
> <br/> 다만, focus 이벤트와 같이 버블링 되지 않는 이벤트도 존재한다.

### 이벤트 캡처링

캡처링은 버블링과는 반대로 최상위 태그에서 이벤트가 발생한 태그를 찾아 내려간다.<br/>
아래와 같이 구현할 수 있지만 자주 사용되는 개념은 아니다.

```javaScript
const divs = document.querySelectorAll("div");

const clickEvent = (e) => {
  console.log(e.currentTarget.className);
}

divs.forEach((div) => {
  div.addEventListener("click", clickEvent, { capture: true });
})
```

`addEventListener`의 옵션 객체에 `{ capture: true }`또는 true를 설정해주면 된다.
캡처링의 경우 `DIV3`을 클릭한다면 `DIV1` -> `DIV2` -> `DIV3`이 순서대로 출력된다.

### 이벤트 전파 막기

코드를 작성하다 보면 이벤트 전파를 막아야 하는 경우가 존재한다.<br/>

`event.stopPropagation()`을 사용하면 클릭한 태그의 이벤트만 발생하고 상위 태그로 이벤트가 전파되는 것을 막을 수 있다.

아래의 코드를 실행하면 `DIV3`의 이벤트만 실행된다.

```javaScript
const clickEvent = (e) => {
    e.stopPropagation();
    console.log(e.currentTarget.className);
}
```

<br/>

참고

- 이벤트 버블링과 캡처링에 대한 정리(https://velog.io/@tlatjdgh3778/%EC%9D%B4%EB%B2%A4%ED%8A%B8-%EB%B2%84%EB%B8%94%EB%A7%81%EA%B3%BC-%EC%BA%A1%EC%B2%98%EB%A7%81%EC%97%90-%EB%8C%80%ED%95%9C-%EC%A0%95%EB%A6%AC)
- 이벤트 버블링, 이벤트 캡쳐 그리고 이벤트 위임까지(https://joshua1988.github.io/web-development/javascript/event-propagation-delegation/)
- 한눈에 이해하는 이벤트 흐름 제어 (버블링 & 캡처링)(https://inpa.tistory.com/entry/JS-%F0%9F%93%9A-%EB%B2%84%EB%B8%94%EB%A7%81-%EC%BA%A1%EC%B3%90%EB%A7%81)
