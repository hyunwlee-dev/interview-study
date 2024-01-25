
출처 : https://poiemaweb.com/es6-iteration-for-of

## 0. iteration protocol의 등장 배경
ES6 이전에는 순회 가능한 데이터 컬렉션(자료구조)가 명확한 통일된 규약 없이 각자의 구조를 갖고 나름의 방식(for...in문으로 순회한다던지...forEach문으로 순회한다던지...)으로 순회할 수 있었다.
그러나 ES6부터는 이 순회 가능한 데이터 컬렉션을 이터레이션 프로토콜을 준수하는 이터러블로 통일하였다. 그리고 이 이터러블이 for...of문, 스프레드 문법, 배열 구조분해할당(destructuring)을 사용할 수 있게 된다. 

<br/>

각각의 순회가능한 자료구조(Array, String, Map/Set 등)를 데이터 공급자 라고 하고,
그것을 순회하여 사용하는 방식(스프레드문법, for..of문, 배열 디스트럭처링 할당 등)을 데이터 소비자라고 하자.

<br/>

만일 각각의 순회가능한 자료구조가 개별의 순회방식을 가지고 있다면, 데이터 소비자는 각 자료구조에 맞게 일일이 순회구조에 맞게 사용방식을 구현해야한다. 
하지만, 이터레이션 프로토콜로 통일된 데이터 공급방식이 제공된다면 데이터 소비자는 이터레이션 프로토콜에 맞게만 구현해놓으면 어떤 순회가능한 자료구조가 오던지 사용할 수 있다. 

<br/>

이렇듯 이터레이션 프로토콜은 데이터 소비자가 효율적으로 데이터 공급자를 사용할 수 있도록 그 사이를 연결하는 인터페이스 역할을 한다.

## 1. iterable
### 1-1. iterable의 정의
iterable protocol을 준수한 객체를 iterable 이라고 한다. 

### 1-2. iterable protocol 이란?
1. Well-Known Symbol*인 Symbol.iterator를 프로퍼티 키로 사용한 메서드를 직접 구현하거나, 프로토타입 체인을 통해 상속받아 갖고 있으며
2.  Symbol.iterator 메서드를 호출하면, iterator protocol을 준수한 iterator를 반환한다. 

### 1-3. iterator란?
iterator protocol을 준수한 객체를 iterator 이라고 한다.

### 1-4. iterator protocol이란?
1. next 메서드를 소유하며
2. next 메서드를 호출하면 value와 done property를 갖는 iterator result 객체를 반환한다.
예시 : { value : 2, done: false }
next메서드를 호출할 때마다 value를 통해 값을 가져오고, 가져올 값이 없으면 done: true를 return한다. 

## 2. Built-In Iterable
자바스크립트에서는 iteration protocol을 준수하는 기본 이터러블을 제공한다. 이를 빌트인 이터러블이라고 한다. 
각 이터러블은, 당연하게도 Symbol.iterator 메서드를 소유하고 있다.

1. Array
2. String
3. Map
4. Set
5. TypedArray*
6. arguments
7. DOM Collection (NodeList, HTMLCollection)*
 

## 3. 이터러블과 유사배열객체와는 다른 것인가?
### 3-1. 유사배열객체란
1) 인덱스로 프로퍼티 값에 접근할 수 있고, 
2) length 프로퍼티를 갖는 객체 이다.
유사배열객체는 for문으로 순회할 수 있으나, 어쨌거나 일반적인 객체이다. 이터러블처럼 Symbol.iterator 메서드를 갖고 있지 않기 때문에 for...of문으로 순회할 수 없다.


### 3-2. 이터러블이면서 유사배열 객체인 것들
- arguments, NodeList, HTMLCollection
ES6가 되면서 위 객체들이 이터러블이 되었다. 
하지만 모든 유사배열객체가 이터러블인 것은 아니다.
반대로, 모든 이터러블이 유사배열객체인것도 아니다. (ex. 배열) 

## 각주
#### Well-Known Symbol
- 자바스크립트가 기본 제공하는 빌트인 심벌값이다. 자바스크립트 엔진의 내부 알고리즘 구현을 위해 사용되는 값이다.

#### TypedArray
- 원시이진데이터를 다루기 위한, 배열처럼 생긴 객체

#### NodeList, HTMLCollection
- DOM API가 결과값을 반환하기 위해 반환하는 유사배열 객체. HTMLCollection은 live 객체이지만 NodeList는 childNodes프로퍼티를 제외하고 Non-live라는 차이가 있다.

## 4. 제너레이터

제너레이터는 **이터레이터를 생성하는 함수** 입니다.

제너레이터 함수는 일반 함수와 다르게 실행 중에 중단할 수 있고, 다시 실행할 수 있는 함수입니다.

제너레이터 함수는 `function*` 키워드로 생성할 수 있으며, `yield`키워드를 사용하여 값을 반환합니다. 

제너레이터 함수를 호출하면 이터레이터 객체가 반환되며, 이터레이터의 `next`메소드를 호출하여 제너레이터 함수를 실행할 수 있습니다.