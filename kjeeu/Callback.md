# 콜백 함수

다른 함수에 인자로 전달되는 함수를 뜻함.
그 함수는 특정 이벤트나 조건이 발생했을 때 실행됨

비동기적인 작업이나 이벤트 처리 등에 사용됨

### 1. 동기와 비동기

동기 : 특정 코드를 수행 완료한 후 다음 코드를 실행

비동기 : 특정 코드를 수행하는 도중 다음 코드를 실행

    ```jsx
    setTimeout(() => {
    	console.log("1시간 후...");
    }, 1000);
    ```

    → 동기적으로 동작하는 자바스크립트에서 비동기적인 수행할 수 있었던 이유는 콜백 함수를 사용했기 때문

### 2. 콜백

    Callback : 되돌아와서 호출

```jsx
const arr = ['겨', '울'];

const print = () => {
    console.log(arr.shift());
    if (!arr.length) {
        clearInterval(timer);
    }
};

const timer = setInterval(print, 1000);
```

setInterval의 인자로 넘긴 print는 콜백 함수.
콜백 함수의 제어권을 넘겨받은 코드는 콜백 함수의 호출 시점에 대한 제어권을 가지게 됨

### 3. 콜백 지옥

비동기 프로그래밍 개발시 발생하는 문제로 함수의 매개 변수로 넘겨지는 콜백 함수가 반복되어 깊어지는 현상
중첩된 콜백 사용시 가독성이 떨어지고 복잡한 코드 구조를 가짐

```jsx
getUserInfo(userId, function (userInfo) {
    getPosts(userInfo, function (posts) {
        displayPosts(posts, function () {
            console.log('포스트를 화면에 표시했습니다.');
        });
    });
});
//각 함수들 비동기적 작동, 콜백 함수를 사용하여 결과 처리
```

### 4. 콜백 지옥 벗어나기

1. Promise 생성 `new Promise(executor)`

    `executor`→ resolve(성공), reject(오류)

    Promise 내부에서 resolve나 reject를 호출해야만 then, catch 부분으로 넘어감 (비동기 작업 → 동기적 표현 가능)

```jsx
getUserInfoPromise(userId)
    .then((userInfo) => getPostsPromise(userInfo))
    .then((posts) => displayPostsPromise(posts))
    .then(() => {
        console.log('포스트를 화면에 표시했습니다.');
    })
    .catch((error) => {
        console.error('오류 발생:', error);
    });
```

2. async/await

    Promise로 비동기 처리 하더라도 콜백 지옥이 될 수 있음

    비동기를 수행하려는 함수 앞에 async 표기
    함수 내부의 실질적인 비동기 필요한 위치에 await 표기
