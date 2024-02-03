# (면접) IIFE 패턴이란 무엇입니까?

IIFE(즉시 실행 함수 표현식)이란 무엇입니까?

IIFE(Immediately Invoked Function Expression)는 정의되자 마자 실행되는 JavaScript 함수입니다.  

```javascript
(function () {
  // login here
})();
```

  

## (꼬리 질문) IIFE 사용 예시

#### 1. 전역 이름공간을 오염시키는 것을 방지

애플리케이션은 다양한 소스 파일의 많은 함수와 전역 변수를 포함할 수 있기 때문에, 전역 변수의 수를 제한하는 것이 중요합니다.  

필요 없는 초기화 코드가 있는 경우, IIFE 패턴을 사용할 수 있습니다. 코드를 다시 재사용하지 않을 것이기 때문에 이 경우 IIFE를 사용하는 것이 함수 선언 또는 함수 표현식을 사용하는 것보다 더 좋습니다.

```javascript
(() => {
  // 초기화 코드
  let firstVariable;
  let secondVariable;
})();

// firstVaribale과 secondVariable은 이 함수 실행 후에 사용할 수 없습니다.
```

  

#### 2. 비동기 함수 실행

`async` IIFE를 사용하면 `top-level await`이 없는 이전 브라우저 및  JavaScript 런타임에서도 `await` 및 `for-await`를 사용할 수 있습니다.  

```javascript
const getFileStream = async(url) => {
  // 구현  
};

(async() => {
  const stream = await getFileStream("https//domain.name/path/file.ext");
  for await (const chunk of stream) {
    console.log({ chunk });
  }
})
```

  

#### 3. 모듈 패턴

IIFE를 사용하는 주된 이유는 IIFE 내에 선언된 모든 변수는 외부 세계에서 액세스 할 수 없기 때문에 데이터 프라이버시를 확보하는 것입니다. 즉, IIFE에서 변수 액세스하려고 하면 아래와 같은 오류가 발생합니다.

```javascript
(function() {
  var message = "IIFE";
  console.log(message);
})();
console.log(message); // Error: message is not defined
```

  

#### 4. ES6 이전의 var가 있는 For 루프

ES6 및 블록 범위에서 let and const문이 도입되기 전에 과거 코드에서 다음과 같은 IIFE 사용을 볼 수 있습니다. var문을 사용하면 함수 범위와 저역 범위만 가지게 됩니다.

###### 의도대로 되지 않은 코드

```javascript
for (var i = 0; i < 2; i++) {
  const button = document.createElement("button");
  button.innerText = `Button ${i}`;
  button.onclick = function () {
    console.log(i);
  };
  document.body.appendChild(button);
}
console.log(i); // 2
```

###### IIFE로 문제 해결한 코드

```javascript
for (var i = 0; i < 2; i++) {
  const button = document.createElement("button");
  button.innerText = `Button ${i}`;
  button.onclick = (function (copyOfI) {
    return function () {
      console.log(copyOfI);
    };
  })(i);
  document.body.appendChild(button);
}
console.log(i); // 2
```
