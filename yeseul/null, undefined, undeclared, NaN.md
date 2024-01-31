# null, undefined, undeclared 그리고 NaN

## null

- null은 **의도적으로 값이 비어있음**을 나타낸다. 그래서 개발자가 변수에 null을 할당하면 의도적으로 값이 없다는 것을 표현할 수 있다.

```
let a = null;
console.log(a); // null
```

- null이 할당된 변수의 타입은 객체(object)이다.

```
console.log(typeof a); // object
```

하지만 null은 javascript의 원시 타입(primitive value) 중 하나이다. 원시 타입의 값은 변경 불가능한 값으로 변경 가능한 값인 객체 타입과는 반대이다. 

그럼에도 불구하고 왜 null의 타입은 object를 반환하는 걸까? 이는 javascript 초기 구현 버그라고 한다.

## undefined

- undefined도 javascript의 원시 타입 중 하나로 변수를 선언은 했지만 **값은 할당하지 않은 상태**이다.
- 선언된 후 값이 할당받지 않은 변수에 자바스크립트 엔진이 암묵적으로 undefined로 초기화시킨다.

```
let b; // 선언은 했지만 값은 할당하지 않음.
console.log(b); // undefined
```

- undefined의 타입은 undefined이다.

```
console.log(typeof b); // undefined
```

- 함수가 값을 return 하지 않는 경우 undefined를 반환한다고 한다. 

```
function foo(x) {
  x * 2;
}
foo(2); // undefined
```

이처럼 함수가 값을 명시적으로 반환하지 않는 경우 undefined를 반환한다.


- var 변수를 호이스팅 하게 되면 선언과 초기화가 동시에 이루어지기 때문에 undefined를 반환한다.

```
console.log(x); // undefined
var x = 1;
```

## undeclared

- 영단어 그대로 **선언되지 않은**, 즉 접근 가능한 스코프에 변수 선언조차 되지 않은 상태로 타입은 undefined이다.

```
console.log(c); // ReferenceError: c is not defined
console.log(typeof c); // undefined
```

javascript가 에러 메시지(ReferenceError) 뿐만 아니라 typeof의 반환값을 전부 undefied로 통일시켜서 실제 typeof가 undeclared인 경우에도 undefined를 반환해 오류 처리를 내뱉지 않는다.

- let, const 선언문을 호이스팅 하게 되면 변수 선언과 초기화가 따로 이루어지기 때문에 변수가 undeclared 상태이므로 ReferenceError가 생긴다.

```
console.log(d); // d is not defined
let d = 1;
```

## NaN

- Not a Number의 약자이며 표현 불가능한 수치형 결과다. **타입은 number**이다.

```
console.log(typeof NaN) // number
```

- NaN은 자기 자신과 일치하지 않는 유일한 값이다.

```
NaN === NaN; // false
NaN !== NaN; // true
```

- 숫자가 NaN 인지 아닌지 판별하려면 isNaN()과 Number.isNaN()을 사용한다. => 인수로 받은 값이 숫자가 아니거나 NaN인 경우 truef를 반환한다.

```
isNaN(NaN); // true
isNaN(10); // false
isNaN(1 + undefined); // true

Number.isNaN(NaN); // ture
```

- isNaN()과 Number.isNaN()의 차이

: isNaN()은 현재 값이 NaN이거나 숫자로 변환했을 때 NaN 이면 true이지만, Number.isNaN은 현재 값이 NaN 이어야만 true를 반환한다. 따라서 isNaN()은 타입 변환을 하지만 Number.isNaN은 타입 변환을 하지 않는다.

```
isNaN("hi omlet"); // true
Number.isNaN("hi omlet"); //false
```

## 면접용 답변
- null : 의도적으로 값이 없다는 것을 명시

- undefined : 개발자가 의도적으로 할당한 값이 아니라 자바스크립트 엔진에 의해 초기화된 값으로 변수를 선언은 했지만 값은 할당하지 않은 상태

- undeclared : 접근 가능한 스코프에 변수 선언조차 되지 않은 상태

- NaN : NaN은 number 타입이지만 숫자가 아님을 의미

## 꼬리 질문 

Q. typeof를 사용해서 null을 확인해보면 어떻게 나오는지 혹시 아시나요?

<details>

<summary>A</summary> 

object가 반환이 되고 이는 자바스크립트의 초기 설계 결함으로 현재까지 수정되지 않은 상태입니다.

</details>

Q. undefined를 개발자가 의도적으로 할당하지 않았는데도 할당된 경우는 어떤 과정에 의해서 그렇게 할당되는 건지 설명해주실 수 있을까요?

<details>

<summary>A</summary> 

개발자가 변수를 선언하게 되면 자바스크립트 엔진은 선언된 변수에 의해 확보된 메모리 공간에 값이 할당될 때까지 내버려 두지 않고(대부분 쓰레기 값(garbage value)이 들어있음) undefined로 초기화합니다.

이처럼 undefined는 null과 달리 개발자가 의도적으로 할당한 값이 아닌 자바스크립트 엔진에 의해 암묵적으로 초기화된 값이므로 의도적으로 값이 없다는 것을 명시하고 싶은 경우는 undefined가 아닌 null을 할당하는 것을 권장합니다. 

</details>

Q. null과 undefined를 느슨한 비교와 엄격한 비교 했을 때 차이에 대해 설명해주세요.

<details>

<summary>A</summary> 

동등 연산자(==)는 자료형이 다르면 자료형을 같게 해서 비교하는 연산자이므로 null과 undefined를 동등 연산자로 비교하면 둘 다 값이 없으니 true가 되지만 자료형까지 비교하는 일치 연산자(===)로 비교하게 되면 null은 의도적으로 빈 값이 들어간 object이므로 false가 됩니다.

</details>

출처
https://velog.io/@lamda/null%EA%B3%BC-undefined-undeclared-%EA%B7%B8%EB%A6%AC%EA%B3%A0-NaN
https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Operators/null
https://velog.io/@fkfnck1532/Javascript-null%EC%9D%98-type%EC%9D%B4-%EA%B3%BC%EC%97%B0-null%EC%9D%BC%EA%B9%8C
https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/undefined
https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/NaNhttps://velog.io/@gonasooc/undefined-null-undeclared%EC%9D%98-%EC%B0%A8%EC%9D%B4%EC%97%90-%EB%8C%80%ED%95%B4-%EC%84%A4%EB%AA%85%ED%95%B4%EC%A3%BC%EC%84%B8%EC%9A%94