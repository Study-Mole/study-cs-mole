# Event Loop

### Event Loop란 무엇인가?

JavaScript는 브라우저 환경이나 Node.js 환경에서 단일 스레드(single-threaded)로 동작하는 언어이다.

즉, 동시에 여러 개의 코드를 병렬로 처리하지 않고, 한 줄씩 순차적으로 실행하는 단일 실행 컨텍스트를 갖는다. 그러나 비동기 처리(예: AJAX 호출, setTimeout 등)를 수행할 때는 단순히 동기적으로 모든 것이 처리되는 것처럼 보이지 않는다.

이 비동기 처리를 가능하게 하는 메커니즘 중 핵심이 바로 **Event Loop(이벤트 루프)** 이다.

**동작 원리 개괄**:

1. **Call Stack(호출 스택)**:

   자바스크립트 엔진이 현재 실행 중이거나 실행 대기 중인 함수들의 스택이다. 함수 호출 시 스택에 쌓이고, 반환 시 스택에서 제거된다.

2. **Memory Heap(메모리 힙)**:

   컴퓨터가 정보를 저장하는곳이다. 즉 자바스크립트 관점에서 보면 우리가 만드는 객체, 배열 ,함수 등의 데이터가 저장되는 공간이다.

3. **Web APIs (또는 브라우저 APIs)**:

   브라우저 환경에서 제공하는 비동기 API들(ex: DOM 이벤트, Ajax, setTimeout, requestAnimationFrame 등)은 호출 스택에 올랐다가 해당 함수가 완료되면 결과를 **Task Queue(또는 Callback Queue)** 에 보낸다.

4. **Task Queue (또는 Callback Queue)**:

   비동기 작업이 완료된 후 호출되어야 할 콜백들이 줄 세워져 있는 대기열이다. setTimeout, network 요청 완료 콜백, DOM 이벤트 핸들러 등이 이 큐에 들어간다.

5. **Microtask Queue (마이크로태스크 큐)**:

   Promise의 `.then()`, `MutationObserver`, queueMicrotask 등을 통해 생성되는 미세한 단위의 비동기 태스크가 대기하는 큐이다. 일반 태스크보다 우선순위가 더 높다.

---

![EventLoop](../Images/event_loop(2).png)

### Event Loop 동작 과정

이벤트 루프는 다음과 같은 과정을 무한 반복한다.

- 호출 스택이 비어 있는지 확인 (또는 비어지기를 기다림)
- 마이크로태스크 큐(Microtask Queue)가 비어있지 않다면 마이크로태스크를 모두 처리
- 그 다음 태스크 큐(Task Queue)에 대기 중인 태스크를 하나 가져와 호출 스택에 올려 실행

이 과정이 끊임없이 반복되면서 JavaScript 엔진은 동기/비동기 작업을 매끄럽게 처리할 수 있다.

**주의사항**:

- **Microtask vs Task**: Microtask는 일반 태스크보다 우선 처리된다. 즉, 한 번의 루프 사이클(틱) 내에서 콜 스택이 비면 바로 Microtask Queue를 체크하고, 모두 처리한 뒤에 다음 Task Queue의 콜백을 처리한다.
- 이 때문에 Promise의 `.then()`이 setTimeout보다 먼저 실행되는 현상이 발생한다.
