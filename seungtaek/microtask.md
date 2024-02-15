### Microtask Queue

1. Microtask Queue 가 뭔가요 ?
   마이크로태스크는 자바스크립트의 이벤트 루프와 관련된 개념 중 하나입니다. 이벤트 루프는 비동기 작업을 처리하고 실행하는 메커니즘을 관리하는데 마이크로태스크는 이 이벤트 루프의 한 부분입니다. </br>
   간단히 말하면 ! 마이크로태스크는 자바스크립트 엔진이 현재 실행 중일 태스크가 완료된 직후에 처리되는 작업을 말합니다. 마이크로태스크 큐에 들어있는 작업은 현재 실행중인 코드의 실행이 끝난 후 즉시 실행됩니다.
   </br>
2. Microtask Queue 가 무엇을 처리하죠 ?
   promise.then, process.nextTick, MutationObserver 와 같이 우선적으로 비동기로 처리되는 함수들의 콜백 함수가 들어가는 큐 (처리 우선순위가 높음)
   </br>

3. Callback Queue의 종류는 ?
   Callback Queue에는 (macro)task queue와 microtask queue 두 가지 종류가 있습니다.
   </br>

4. 그렇다면 Callback Queue의 Call Stack 으로 옮겨지는 순서는 어떻게 되나요 ?
   microtask queue가 가장 우선순위가 높아 먼저 microtask queue를 처리하여 먼저 비우고 그라음 task queue의 콜백을 처리한다.
   </br>

5. (macro)task queue 는 무엇을 처리하죠 ?
   Task Queue : setTimeout, setInterval, fetch, addEventListener 와 같이 비동기로 처리되는 함수들의 콜백 함수가 들어가는 큐 (macrotask queue 는 보통 task queue 라고 부릅니다.)

### ref

[Inpa-dev](https://inpa.tistory.com/entry/%F0%9F%94%84-%EC%9E%90%EB%B0%94%EC%8A%A4%ED%81%AC%EB%A6%BD%ED%8A%B8-%EC%9D%B4%EB%B2%A4%ED%8A%B8-%EB%A3%A8%ED%94%84-%EA%B5%AC%EC%A1%B0-%EB%8F%99%EC%9E%91-%EC%9B%90%EB%A6%AC)
[코어-자바스크립트](https://ko.javascript.info/microtask-queue)
[Frontend-Interview-Questions](https://github.com/Esoolgnah/Frontend-Interview-Questions/blob/main/Notes/important-4/microtask-queue-task-queue.md)
