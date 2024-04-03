# Adding Interactivity

## Responding to Events
- `<button>` 같은 요소에 `prop`으로 함수를 전달함으로써 이벤트를 핸들링할 수 있다.
- 이벤트 핸들러는 호출이 아닌 전달되어야 한다.
- 이벤트 핸들러 함수는 개별적으로 또는 인라인 형태로 정의할 수 있다.
- 이벤트 핸들러는 컴포넌트 안에 정의되고, 이는 `props`에 접근할 수 있다.
- 부모 안에 선언된 이벤트 핸들러는 자식에게 `prop`으로 전달될 수 있다.
- 사용자 정의 이벤트 핸들러에 앱에 특화된 이름을 부여할 수 있다.
- 이벤트는 위로 전파되는데 첫 번째 매개변수로`e.stopPropagation()`을 호출해서 전파를 방지할 수 있다.
- 이벤트는 원치 않는 기본 브라우저 동작을 유발할 수 있고, 이를 방지하기 위해 `e.preventDefault()`를 호출한다.
- 명시적으로 이벤트 핸들러 `prop`을 자식 핸들러에서 호출하는 것은 전파에 대한 좋은 대안이 될 수 있다.

## State: A Component's Memory
- state 변수를 사용하는 것은 컴포넌트가 렌더링 사이에서 어떤 정보를 기억할 필요가 있을 때이다.
- state 변수는 `useState` Hook을 호출함으로써 선언된다.
- Hook은 `use`로 시작하는 특별한 함수이며, state 같은 리액트 기능을 연결시켜 준다.
- Hook은  무조건 호출되어야 하는 import를 상기시키고, `useState`를 포함한 Hook을 호출하는 것은 컴포넌트의 최상단이나 다른 Hook에서만 유효하다.
- `useState` Hook은 현재 state와 이를 업데이트하는 함수로 이루어진 한 쌍의 값을 반환한다.
- 하나 이상의 state 변수를 가질 수 있고, 내부적으로 리액트는 이러한 state 변수를 순차적으로 매칭한다.
- 컴포넌트 안에서 state는 프라이빗하고, 만약 이를 두 곳에서 렌더링 한다면, 각각의 복사본은 고유한 state를 얻는다.

## Render and Commit
- 리액트 앱이 화면 업데이트하는 세 가지 방법
  1. Trigger
  2. Render
  3. Commit
- 엄격 모드에서 컴포넌트 안의 실수를 찾을 수 있다.
- 리액트는 마지막 렌더링 결과와 같으면 DOM을 건드리지 않는다.

## Satate as a Snapshot
- `state` 설정은 새로운 렌더링을 요청하는 것이다.
- 리액트는 컴포넌트 밖에서 `state`를 선반에 두는 것처럼 저장한다.
- 리액트는 `useState`를 호출하면 렌더링을 위한 `state`의 스냅샷을 제공한다.
- 변수와 이벤트 핸들러는 리렌더링 시에 살아남지 않고, 모든 렌더링은 고유한 이벤트 핸들러를 가진다.
- 모든 렌더링(과 그 안의 함수)은 항상 리액트가 렌더링 하기 위해 제공하는 `state`의 스냅샷을 본다.
- 렌더링 된 JSX에 대해 생각하는 방식과 유사하게 이벤트 핸들러에서 `state`를 대체할 수 있다.
- 과거에 만들어진 이벤트 핸들러는 그 시점은 `state` 값을 갖는다.

## Queueing a Series of State Updates
- `state`를 설정하는 것은 기존 렌더링 변수를 변경하는 것이 아닌 새로운 렌더링을 요청하는 것이다.
- 리액트는 이벤트 핸들러 동작이 다 끝난 후에 `state` 업데이트를 진행하며 이를 `batching`이라 한다.
- 하나의 이벤트 안에서 일부 `state`를 여러 번 업데이트하기 위해 `setNumber(n => n + 1)` 과 같은 업데이트 함수를 사용할 수 있다.

## Updating Objects in State
- 리액트는 모든 `state`를 불변한 것으로 다룬다.
- `state`안에서 객체를 저장할 때, 객체의 변화는 렌더링을 유발하지 않고 이전의 렌더링 스냅샷에서 `state`가 변화한다.
- 객체를 변화시키는 것 대신에, 새로운 버전의 객체를 생성하면 객체의 `state` 설정에 의해서 리렌더링이 발생한다.
- 스프레드 문법은 객체 복사본을 생성한다.
- 스프레드 문법은 1depth만 복사하는 얕은 복사이다.
- 중첩 객체를 업데이트하기 위해, 업데이트된 부분부터 끝까지 객체 복사본을 생성해야 한다.
- 반복적인 복사 코드를 줄이기 위해서 `Immer`를 사용한다.

## Updating Arrays in State
- 배열을 `state`로 만들 수 있지만 변경은 불가능하다.
- 배열을 변경하는 것 대신에 새로운 버전의 배열을 생성하고 `state`를 업데이트한다.
- 배열 스프레드 문법을 사용해서 새로운 항목을 포함한 배열을 생성한다.
- 변화되거나 필터링 된 항목으로 새 배열을 생성하기 위해 `filter()`와 `map()`을 사용한다.
- 코드의 간결함을 유지하기 위해 `Immer`를 사용한다.