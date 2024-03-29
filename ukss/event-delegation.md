---
title: event-delegation
date: 2024-01-25
description: 이벤트 위임 면접질문 정리
---

### 이벤트 버블링 (Event Bubbling)

1. 이벤트 버블링이 뭔가요?

    이벤트를 처리할 때 여러 요소에서 이벤트를 처리할 수 있는데 이 때 **이벤트가 일어난 요소에서 시작해 최상단 document까지 거슬러 올라가는 방법**을 말합니다.
    <br />

2. 이벤트 버블링의 예시

    body -> div -> p 의 레이아웃의 경우, p에서 이벤트가 발생할 경우 상위 요소까지 이벤트가 전파가 되어 p, div, body 요소들에서 해당 이벤트가 작동됩니다. 이러한 이벤트가 발생한 요소부터 상위 요소로 전파되는 특성을 **이벤트 버블링**이라고 합니다.

### 이벤트 캡처링 (Event Capturing)

1. 이벤트 캡처링이 뭔가요?

    이벤트가 특정 요소에서 발생하였을 때, 해당 이벤트가 발생하는 위치뿐만 아니라 **하위 요소들로 전파되는 특성**을 의미합니다.
    <br />

2. 이벤트 캡처링의 예시

    body -> div -> p 의 레이아웃의 경우, body에서 이벤트가 발생할 경우 하위 요소까지 이벤트가 전파가 되어 body, div, p 요소들에서 해당 이벤트가 작동됩니다. 이러한 이벤트가 발생한 요소부터 하위 요소로 전파되는 특성을 **이벤트 캡처링**이라고 합니다.

### 이벤트 위임 (Event Delegation)

1. 이벤트 위임이 뭔가요?

    이벤트 위임은 캡쳐링과 버블링을 활용하여 만든 강력한 이벤트 핸들링 패턴이다. 이벤트 위임을 사용하면 여러 요소마다 각각 이벤트 핸들러를 할당하지 않고, 요소의 공통 조상에 이벤트 핸들러를 단 하나만 할당해도 여러 요소를 한꺼번에 다룰 수 있다.
    <br />

2. 이벤트 위임의 이점

    1. 동적인 요소에 대한 이벤트 처리가 수월하다.
    2. 상위 요소에서만 이벤트 리스너를 관리하기 때문에 하위 요소는 자유롭게 추가 삭제할 수 있다.
    3. 이벤트 핸들러 관리가 쉽다.
    4. 동일한 이벤트에 대해 한 곳에서 관리하기 때문에 각각의 엘리먼트를 여러 곳에 등록하여 관리하는 것보다 관리가 수월하다.
    5. 메모리 사용량이 줄어든다. 또한 메모리 누수 가능성도 줄어든다.
       <br />

### ref

-   [버블링과 캡처링](https://ko.javascript.info/bubbling-and-capturing)
-   [이벤트 위임](https://ko.javascript.info/event-delegation)
-   [[JavaScript] 이벤트 위임 - moonee](https://velog.io/@moonheekim0118/JavaScript-%EC%9D%B4%EB%B2%A4%ED%8A%B8-%EB%B2%84%EB%B8%94%EB%A7%81)
-   [왜 이벤트 위임(delegation)을 해야 하는가?](https://ui.toast.com/weekly-pick/ko_20160826)
