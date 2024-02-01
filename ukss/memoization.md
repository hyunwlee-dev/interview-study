---
title: memoization
date: 2024-01-31
description: memoization 면접질문 정리
---

### 메모이제이션 (Memoization)

1. 메모이제이션이 무엇인가요?

    - 컴퓨터 프로그램이 동일한 계산을 반복해야 할 때 이전에 계산한 값을 메모리에 저장함으로써 동일한 계산의 반복 수행을 제거하여 프로그램 실행 속도를 빠르게 하는 기술이다. 동적 계획법의 핵심이 되는 기술이다.
    - 이름대로 **메모**를 하는 것이 특징인데, 프로그래밍에서 **반복되는 결과를 메모리에 저장**하고 다음에 같은 결과가 나올 때 다시 계산할 필요없이 저장된 결과를 실행하는 기법이다. 마치 **캐싱**과 같은 기능이라고 할 수 있다.
      <br /><br />

2. Javascript에서는 어떻게 사용될 것인가.

    - Javascript에서는 **클로저**를 통해 계속 유지되는 저장공간을 만들 수 있기 때문에, 이것을 이용하면 메모이제이션 패턴을 구현할 수 있다.

    - 예시로 일반적인 재귀함수로 **피보나치 수열**을 계산하는 함수를 구현하려고 한다면 다음과 같이 진행할 수 있다.
      피보나치 수열은 바로 앞 두 항의 합으로 이루어진 수열이다. e.g. 0, 1, 1, 2, 3, 5, 8 •••

        ```jsx
        let count = 0;

        const fibonacci = (number) => {
            count++;
            return number < 2 ? number : fibonacci(number - 1) + fibonacci(number - 2);
        };

        for (let i = 0; i <= 10; i++) {
            console.log(fibonacci(i));
        }

        console.log('count : ' + count);
        ```

        ```zsh
        ➜  test-js node "/Users/ukss/Documents/test-js/app.js"
        0
        1
        1
        2
        3
        5
        8
        13
        21
        34
        55
        count : 453
        ```

        - 위의 예시 처럼 반복문으로 구현은 가능하지만 파라미터로 10이라는 비교적 작은 수를 주었지만 fibonacci 함수는 **453번**이나 실행된다. 여기서는 11번은 직접 호출한 것이지만, 나머지 442번은 이미 계산한 값들을 다시 계산하기 위해 호출한 것이다.
        - number가 4일 때 경우에 이전에 계산한 값을 다시 계산하는 패턴을 확인할 수 있다.

        ```jsx
        fibonacci(0) = fibonacci(0)
        fibonacci(1) = fibonacci(1)
        fibonacci(2) = fibonacci(0) + fibonacci(1) = fibonacci(0) + fibonacci(1)
        fibonacci(3) = fibonacci(1) + fibonacci(2) = fibonacci(1) + (fibonacci(0) + fibonacci(1))
        fibonacci(4) = fibonacci(2) + fibonacci(3) = (fibonacci(0) + fibonacci(1)) + (fibonacci(1) + (fibonacci(0) + fibonacci(1)))
        ```

        - 이때, 반복되는 패턴을 메모이제이션 패턴으로 이용하면 불필요한 연산을 줄일 수 있다.

        ```jsx
        let count = 0;
        const fibonacci = (function () {
            const memo = [0, 1];
            const fib = function (number) {
                count++;
                let result = memo[number];
                if (typeof result !== 'number') {
                    result = fib(number - 1) + fib(number - 2);
                    memo[number] = result;
                }
                return result;
            };
            return fib;
        })();

        for (let i = 0; i <= 10; i++) {
            console.log(fibonacci(i));
        }

        console.log('count : ' + count);
        ```

        ```zsh
        ➜  test-js node "/Users/ukss/Documents/test-js/app.js"
        0
        1
        1
        2
        3
        5
        8
        13
        21
        34
        55
        count : 29
        ```

        - 이전에 453번 실행되던 것이 **29번**으로 줄어든 것을 확인할 수 있는데, 이는 불필요한 연산을 줄였기 때문이다. 29번 중 11번은 이전과 똑같이 직접 호출한 것이고 18번은 메모이제이션 결과를 얻기 위해 호출한 것이다.
        - 전자의 경우 1에서 10까지 모든 수를 비교하기 위해 fibonacci 함수가 일일이 호출되었지만, 다음과 같은 메모이제이션 패턴에서는 **memo**라는 배열을 만들어 계산한 값을 저장해두고 그 배열을 클로저를 통해 접근한다.
        - 로직을 처리하는 클로저가 반복수행하기에 더 빠르게 처리할 수 있는 것이다. 연산해야할 값이 더 클수록 성능은 크게 차이가 날 것이다.
          <br /><br />

3. 리액트에서 제공하는 메모이제이션 기법이 무엇이 있을까요?

    `React.memo(컴포넌트)`, `useCallback(() => {함수 그 자체...}, [])`, `useMemo(() => 함수의 리턴 값, [])`
    <br /><br />

4. react의 메소드들은 어떤 식으로 메모이제이션을 하고 있을까?
    1. `React.memo()`
        - **컴포넌트 자체**를 메모이제이션하는 기법이다.
        - **자체적으로 props값을 비교**해서 달라진 부분이 없다면 리액트 DOM에서 비교 작업이 발생하지 않는다.
          <br /><br />
    2. `useCallback()`
        - **함수 자체**를 메모이제이션하는 기법이다.
        - Javascript에서 함수는 객체로 취급되기 때문에 리렌더링이 발생할 때마다 새로운 함수가 생성된다.
        - 자식 컴포넌트의 불필요한 리렌더링을 막기 위해서는 useCallback()으로 감싸줘야 한다.
        - **deps에 들어있는 의존성 값**이 변경되지 않는다면 이전에 생성한 함수의 참조 값을 반환해준다.
          <br /><br />
    3. `useMemo()`
        - **함수의 리턴 값**을 메모이제이션하는 기법이다.
        - **deps에 들어있는 의존성 값**이 변경되지 않는다면 메모이제이션된 값을 사용한다.
          <br /><br />
    4. **위 함수들의 공통점**
        - 리렌더링이 자주 발생하지 않는다면 굳이 사용할 필요가 없다. (메모리에 불필요하게 남아있을 필요가 없기 때문)
        - props나 state가 변경되는 경우가 대부분일 경우 굳이 비교 작업을 계속할 필요가 없기 때문에 사용할 필요가 없다.
        - state나 props의 값이 어느 정도 적당히 변경되는 경우 사용해주면 효율적이다.

<br /><br />

### ref

-   [메모이제이션 - 위키백과](https://ko.wikipedia.org/wiki/%EB%A9%94%EB%AA%A8%EC%9D%B4%EC%A0%9C%EC%9D%B4%EC%85%98)
-   [메모이제이션(Memoization)이란? - nanana.log](https://velog.io/@4775614/%EB%A9%94%EB%AA%A8%EC%9D%B4%EC%A0%9C%EC%9D%B4%EC%85%98Memoization%EC%9D%B4%EB%9E%80)
-   [메모이제이션 (Memoization) - da.som](https://frontsom.tistory.com/11)
-   [[React] 리액트에서의 메모이제이션 (react memoization) - 밀가루의 개발일기](https://bbangaro.tistory.com/66)
