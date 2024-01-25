## Rest 연산자

- 나머지라는 의미 ⇒ 나머지 요소들을 뭉쳐준다!
- 객체의 나머지 요소들, 배열의 나머지 요소들
- 함수 파라미터들의 리스트

### 배열에서의 …rest

```jsx
const numbers = [0, 1, 2, 3, 4, 5, 6];

const [one, ...rest] = numbers;

console.log(one);
console.log(rest); // [1, 2, 3, 4, 5, 6]
```

### 객체에서의 …rest

```jsx
const user = {
	name: 'dr',
	age: 30,
	address: 'seoul',
};

const {name, ...rest} = user;

console.log(name)
console.log(rest) // 어떤 타입이 나올까요?
```

- 구조분해할당과 같이 많이 사용된다.

### 함수 파라미터에서의 …rest

```jsx
function foo(a, b, ...rest){
	return (a * b) + rest.reduce((acc, curr) => acc + curr, 0); 
}
```

- 이전에는 arguments ‘객체’를 사용했었는데 그러다보니 배열로 바꾸는 한번 작업이 필요했다.
- rest 연산자가 등장하면서 바로 배열 메소드들을 사용할 수 있게 되었다.

---

## Spread 연산자 (전개 연산자)

- 펼친다 라는 의미
- **객체나 배열의 요소를 하나씩 펼친다** 라고 생각하면 된다.
- 스프레드 문법을 사용할 수 있는 대상은 **이터러블에 한정된다.**
    - 그러나 객체 리터럴에서도 ‘스프레드 프로퍼티’ 문법을 통해 사용할 수 있게 개정되었다.

### 배열에서의 Spread연산자

- 배열 합치기

```jsx
const arr = [1,2,3];
const arr2 = [4,5,6];

const arr3 = [...arr, ...arr2];
```

- 배열 복사

```jsx
const arr = [1,2,3]
const copy = [...arr]
```

- 이터러블을 배열로 변환

```jsx
// 이터러블이면서 유사배열 객체인 arguments객체를 배열로 변환
function sum(){
	return [...arguments].reduce((acc, curr) => acc + curr, 0);
}

sum(1, 2, 3, 4) // 10
```

### 객체에서의 Spread연산자

- 일반 객체에서도 사용가능한 스프레드 문법

```jsx
const obj = {x: 1, y: 2}
const copy = {...obj}

console.log(obj === copy) // ?
```

### 함수에서의 Spread연산자

```jsx
const arr = [1, 4, 5, 8, 10, 3, 2, 9];

Math.max(...arr); // 10
```

- rest 때와는 반대로 요소 하나씩을 파라미터로 함수에 넣어줄 수 있다.

---

## Destructuring (구조분해 할당, 비구조화 할당)

- ‘분해’ 라는 말은 파괴보다는 변수로 ‘나눈다’ 라는 의미에 더 가깝다.
- 배열 구조분해할당의 우변은 이터러블이어야하며, 할당 기준은 배열의 인덱스 이다.

### 배열 구조분해할당

네 번째 요소도 존재하지만, 변수를 할당하지 않았기 때문에 이 구문으로는 별도 변수로 불러올 수 없다.

```jsx
let [firstName, , title] = ["Julius", "Caesar", "Consul", "of the Roman Republic"];
```

- 배열 구조분해 할당에서의 기준은 인덱스이다.
- 필요하지 않은 요소는 쉼표로 버릴 수 있다.

### 객체 구조분해할당

```jsx
const odd = {one: 1, three: 3};
const {one, three} = odd;

console.log(one + three) // 4
```

- 객체 구조분해 할당에서의 기준은 프로퍼티 키이다.

```jsx
const {one: first, three: third} = odd;

console.log(first + third) // 4
```

- 원하는 변수명을 적용하기 위해서는 다음과 같이 변수를 선언해야한다.