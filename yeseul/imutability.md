# 불변성(immutability)을 유지하는 방법

- 객체 타입(Object Type)의 데이터는 변경될 수 있어서 예상치 못한 오류가 발생할 수 있다. 
```
let x = {
  name: "egg"
};

let y = x;
y.name = "omlet";

console.log(x.name); // omlet
```

## 불변성(immutability)을 유지하는 방법
1. 깊은 복사
  1) Object.assign()
  - Object.assign(target, source)
    - target : 새롭게 만들어지는 객체
    - source : 복사의 대상이 되는 객체

  ```
  let x_copy1 = {
    name: "egg"
  };

  let y_copy1 = Object.assign({}, x_copy1);
  y_copy1.name = "omlet";

  console.log(x_copy1.name); // egg

  ```

  2) spread 연산자
  - spread 연산자는 복사하고자 하는 객체 앞에 점 세개, ...을 붙임
  - Spread로 복사한 객체는 1depth의 값에서만 깊은 복사를 실행한다.
  
  ```
  let x_copy2 = {
    name: "egg",
    form: {
        color: "white",
    }
  };

  let y_copy2 = {...x_copy2};
  y_copy2.name = "omlet";
  y_copy2.form.color = "yellow";

  console.log(x_copy2.name); // egg
  console.log(x_copy2.form.color); // yellow
  ```

2. 객체 자체를 불변하게 만들기(preventExtensions < seal < freeze)

  1) Object.preventExtensions()
  - 갹체의 확장을 막는 메서드로 새로운 속성을 추가하려고 시도하면 TypeError가 발생하거나 조용히 실패하게된다. 
  - 기존의 값을 변경하거나 delete 연산자를 통해서 속성을 삭제 가능
  
  ```
  const coordinate1 = {
    x: 10,
    y: 20
  };

  Object.preventExtensions(coordinate1);

  Object.defineProperty(coordinate1, "z", {
      value: 30,
  }); // TypeError: Cannot define property z, object is not extensible
  console.log(coordinate1); // {x: 10, y: 20}

  Object.defineProperty(coordinate1, 'x', {
      value: 15,
  });
  console.log(coordinate1); // {x: 15, y: 20}

  delete coordinate1.y;
  console.log(coordinate1); // {x: 15}
  ```

  2) Object.seal()
  - 속성의 추가와 삭제를 둘 다 막는 메서드
  - 기존의 속성 값 변경은 가능

  ```
  const coordinate2 = {
    x: 10,
    y: 20
  };

  Object.seal(coordinate2);

  Object.defineProperty(coordinate2, "z", {
      value: 30,
  }); // TypeError: Cannot define property z, object is not extensible
  console.log(coordinate2); // {x: 10, y: 20}

  Object.defineProperty(coordinate2, 'x', {
      value: 15,
  });
  console.log(coordinate2); // {x: 15, y: 20}

  delete coordinate2.y; // silent fail
  console.log(coordinate2); // {x: 15, y: 20}
  ```

  3) Object.freeze()
  - 속성의 추가, 삭제, 값 변경을 모두 막는 강력한 메서드
  - 2단계 이상의 객체에는 적용 불가

  ```
  const coordinate3 = {
    x: 10,
    y: 20
  };

  Object.freeze(coordinate3);

  Object.defineProperty(coordinate3, "z", {
      value: 30,
  }); // TypeError: Cannot define property z, object is not extensible
  console.log(coordinate3); // {x: 10, y: 20}

  Object.defineProperty(coordinate3, 'x', {
      value: 15,
  }); // TypeError: Cannot redefine property: x
  console.log(coordinate3); // {x: 10, y: 20}

  delete coordinate3.y; // silent fail
  console.log(coordinate3); // {x: 10, y: 20}
  ```

- Object.isSealed()
  - Object.seal() 메서드 적용 여부 확인
  - 만약 Object.freeze() 메서드가 적용되어 있다면, freeze의 효과가 seal을 포함하고 있기 때문에 true반환

  ```
  Object.isSealed(coordinate1); // false
  Object.isSealed(coordinate2); // true
  Object.isSealed(coordinate3); // true
  ```

- Object.isFrozen()
  - Object.freeze() 메서드 적용 여부 확인

  ```
  Object.isFrozen(coordinate1); // false
  Object.isFrozen(coordinate2); // false
  Object.isFrozen(coordinate3); // true
  ```

## 면접용 답변
- 불변성을 지켜야하는 이유는?
: 원본데이터가 변경될 경우 해당 데이터를 참조하고 있는 다른 객체에서 예상치 못한 오류가 발생할 수 있기 때문입니다. 이처럼 데이터를 변경하지 않고 새로운 데이터를 생성하며 코드를 작성하면 예측 가능하고 성능 최적화된 애플리케이션을 구축할 수 있어 보다 견고하고 유지보수가 용이한 코드를 작성할 수 있습니다.

- 불변성(immutability)을 유지하는 방법은?
: 깊은 복사 방법인 Object.assign() 메서드와 스프레드 문법이 있고, 객체 자체를 불변하게 만드는 preventExtensions(), seal(), freeze() 메서드가 있고, 불변성 라이브러이인 immer와 Immutable, 마지막으로 map, filter, slice, reduce와 같은 새로운 배열을 반환하는 메서드를 사용하여 불변성을 유지할 수 있습니다.

## 꼬리 질문
Q. preventExtensions(), seal(), freeze() 의 차이를 설명해주세요.

<details>

<summary>A</summary> 

freeze() 메서드는 속성의 추가, 삭제, 값 변경을 모두 막고 seal() 메서드는 속성의 추가, 삭제는 막지만 값의 변경은 가능하고 preventExtensions() 메서드는 속성 추가만 막는 메서드로 값의 변경과 삭제는 가능합니다.

</details>

Q. const도 불변인가요?

<details>

<summary>A</summary> 

const도 재선언 및 재할당이 불가능하긴 하지만, 값에 대한 참조가 한번 변수에 할당되면 변할 수 없음을 의미하는 것이지 const변수가 참조하고 있는 값이 불변한다는 의미는 아닙니다.

</details>

출처: 
https://hyeok1235.tistory.com/35
https://velog.io/@saiani1/CS%EC%8A%A4%ED%84%B0%EB%94%94-%EC%9E%90%EB%B0%94%EC%8A%A4%ED%81%AC%EB%A6%BD%ED%8A%B8-%EB%B6%88%EB%B3%80%EC%84%B1%EC%9D%84-%EC%9C%A0%EC%A7%80%ED%95%98%EB%8A%94-%EB%B0%A9%EB%B2%95-%EB%A6%AC%EC%95%A1%ED%8A%B8-Props-Drilling%EC%9D%B4%EB%9E%80