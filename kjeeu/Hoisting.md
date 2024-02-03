# 호이스팅

```jsx
console.log(a); //Reference Error
const a = 2;
```

인터프리터가 코드를 실행하기 전에 변수 및 함수의 선언문이 스코프 내의 상단으로 끌어올려지는 것 같은 현상

→ 실행 컨텍스트 생성 시 렉시컬 스코프 내의 선언이 끌어올려 지는 것 같은 현상

-   실행 컨텍스트
    코드가 실행될 때 생성되는 환경
    변수 객체, 스코프 체인, this 바인딩
-   렉시컬 스코프
    함수를 어디서 호출하는지가 아닌 어디에 선언했는지에 따라 상위 스코프를 결정하는 것
    ```jsx
    var test = 1;

    function first() {
        var test = 100;
        second();
    }

    function second() {
        console.log(test);
    }

    first();
    second();
    ```

`const a` 는 변수 선언문으로 컴파일시 선언 단계만 호이스팅됨 (const, let)

var는 선언과 동시에 초기화가 이루어지므로 undefined 출력

```jsx
test(); //함수 호이스팅
function test() {
    console.log('함수 호이스팅');
}
```

함수선언문의 경우 나중 사용을 위해 저장되며 call 될 때 실행됨

함수표현식은 호이스팅 되지 않음
