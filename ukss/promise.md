---
title: promise
date: 2024-01-25
description: promise 면접질문 정리
---

### promise

1. promise가 무엇인가요?

    자바스크립트 비동기 처리에 사용되는 객체, 비동기 처리의 단점을 보완하여 동기적으로 처리할 수 있게 도와준다.
    <br />

2. 비동기처리가 무엇인가요?

    특정 코드의 실행이 완료될 때까지 기다리지 않고 다음 코드를 먼저 수행하는 자바스크립트의 특성을 말합니다.
    요청 후 정상처리 되기도 전에 화면에 빈 데이터를 표시하는 상태
    <br />

3. promise가 필요한 이유는 무엇인가요?

    fetch 등으로 서버에서 데이터를 요청하고 받아온 뒤 이를 처리하기 위하여 사용됩니다.
    <br />

4. promise를 사용함으로써 얻는 장점이 무엇이 있나요?

    promise는 .then(), .catch() 등 경우에 따른 성공, 오류처리가 가능하며 기존 콜백 함수 방식보다 코드의 양을 줄여 가독성을 증가시킬 수 있습니다.
    <br />

5. promise의 3가지 상태에 대해 이야기 해주세요.

    1. pending: 대기, 비동기 처리 로직이 아직 완료되지 않은 상태
    2. fulfilled: 이행, 비동기 처리가 완료되어 프로미스가 결과 값을 반환해준 상태
    3. rejected: 실패, 비동기 처리가 실패하거나 오류가 발생한 상태
       <br />

6. 콜백함수 방식은 무엇인가요? 이에 발생하는 단점도 이야기 해주세요

    callback함수는 다른 함수의 인수로 넘겨줌으로써(두번째 인자로 콜백을 넣음) 실행이 가능한 코드를 말합니다. callback을 반복적으로 실행시키기 때문에 코드의 양이 길어지고 콜백지옥과 같은 현상이 발생하며 비교적 가독성이 떨어진다는 단점이 있습니다.
    <br />

7. promise와 callback의 차이점을 설명해주세요.

    - callback함수는 함수안에서만 결과값 처리와 이에 따른 결과값을 알 수 있지만, promise는 비동기 로직에서 처리된 결과값이 promise객체에 저장되기 때문에 로직 밖에서도 사용이 가능합니다.
    - callback함수는 함수 내부에서 계속해서 연달아 호출하므로 가독성이 떨어지지만, promise함수는 promiseAPI를 사용하여 가독성을 증가시킬 수 있습니다.

### ref

-   [프라미스와 async, await](https://ko.javascript.info/promise-basics)
-   [자바스크립트에서 비동기 처리 다루기 - Promise](https://learnjs.vlpt.us/async/01-promise.html)
-   [자바스크립트 Promise 개념 & 문법 정복하기](https://inpa.tistory.com/entry/JS-%F0%9F%93%9A-%EB%B9%84%EB%8F%99%EA%B8%B0%EC%B2%98%EB%A6%AC-Promise)
-   [기술문제 면접(promise(프로미스)의 개념에 대해서 설명하고, 콜백 함수 방식과 차이점을 설명해주세요.)](https://velog.io/@solimlee/%EA%B8%B0%EC%88%A0%EB%AC%B8%EC%A0%9C-%EB%A9%B4%EC%A0%91promise%ED%94%84%EB%A1%9C%EB%AF%B8%EC%8A%A4%EC%9D%98-%EA%B0%9C%EB%85%90%EC%97%90-%EB%8C%80%ED%95%B4%EC%84%9C-%EC%84%A4%EB%AA%85%ED%95%98%EA%B3%A0-%EC%BD%9C%EB%B0%B1-%ED%95%A8%EC%88%98-%EB%B0%A9%EC%8B%9D%EA%B3%BC-%EC%B0%A8%EC%9D%B4%EC%A0%90%EC%9D%84-%EC%84%A4%EB%AA%85%ED%95%B4%EC%A3%BC%EC%84%B8%EC%9A%94)
