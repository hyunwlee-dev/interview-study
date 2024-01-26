# this

: this는 자신이 속한 객체 또는 자신이 생성할 인스턴스를 가리키는 자기 참조 변수

## 함수 호출 방식과 this 바인딩

: this 바인딩은 함수 호출 방식에 의해 동적으로 결정된다. 

1. 일반 함수

- 기본적으로 전역 객체가 바인딩된다.

```
function a() {
  console.log(this);
}
a(); // Window {}
```

- stridt mode(엄격 모드)에서는 함수 내의 this에 디폴트 바인딩에 없어서 undefined가 된다. 

```
'use strict';

funtion a() {
  console.log(this);
}
a(); // undefined
```

​- 중첩 함수 또는 콜백 함수는 외부 함수를 돕는 콜백 함수 역할을 하므로 이러한 함수의 this 바인딩은 외부 함수의 this 바인딩과 일치하지 않아 콜백함수로 동작하기 어렵다. 

  - 해결 방법

  1. this 바인딩을 변수 that에 할당해서 this 대신 that을 참조.
  2. 화살표 함수 사용
  3. apply, call, bind 메서드 사용

2. 메서드

- 메서드를 호출한 객체가 바인딩된다.

```
var obj = {
  a: function() {
    console.log(this);
  }
};
obj.a(); // {a: ƒ}
```

3. 생성자 함수

- 생성자 함수가 미래에 생성할 인스턴스가 바인딩된다.

```
function Person(name) {
  this.name = name;
}
 
var omlet = new Person('omlet');

console.log(omlet.name); // omlet
```

- new  연산자와 함께 생성자 함수를 호출하지 않으면 일반 함수로 동작해 this가 Window에 바인딩 된다.

```
function Person(name) {
  this.name = name;
}
 
var omlet = Person('omlet');

console.log(window.name); // omlet
```

4. apply, call, bind 메서드

- apply와 call 메서드를 사용해 arguments 객체와 같은 배열이 아닌 유사 배열 객체에 Array.prototype.slice 같은 배열의 메서드를 사용할 수 있다. 
- bind 메서드는 메서드의 this와 내부의 중첩 함수 또는 콜백 함수의 this가 불일치하는 경우 해결

- apply
  - 첫 번째 인자는 this에 바인딩 할 객체
  - 두 번째 인자부터 인수를 배열로 받음
  - 함수를 호출하는 기능

```
function foo(a,b) {
  console.log(this.name, a, b);
}
const obj = {
  name: 'omlet',
};
foo.apply(obj, [1, 2]); // omlet 1 2
```

- call
  - 첫 번째 인자는 this에 바인딩 할 객체
  - 두 번째 인자부터 인수를 콤마로 구분해 받음
  - 함수를 호출하는 기능

```
function foo(a,b) {
  console.log(this.name, a, b);
}
const obj = {
  name: 'omlet',
};
foo.call(obj, 1, 2); // omlet 1 2
```

- bind
  - 첫 번째 인자는 this에 바인딩 할 객체
  - 새로운 함수를 반환하는 함수

```
function foo(a,b) {
  console.log(this.name, a, b);
}
const obj = {
  name: 'omlet',
};
const binding = foo.bind(obj, 1);
binding(2); // omlet 1 2

```

5. 화살표 함수

- 상위 스코프의 this 값을 가리킨다.

```
const person = {
  name: 'omlet',
  foo: () => {
    console.log(this.name);
  }
}
person.foo(); // omlet
```

## 함수에 명시적으로 this를 바인딩 할 수 있는 방법 3가지

1. map, forEach 같은 고차 함수 사용

2. apply, call, bind 메서드 사용

3. 화살표 함수 사용

출처

모던 자바스크립트 Deep Dive