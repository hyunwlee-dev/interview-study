# React에서의 ref는 무엇인가요? (사용하는 이유는 무엇인가요?)

>  ref는 **DOM을 직접적으로 건드려야하는 때** 사용합니다. 순수 js로 컴포넌트를 제어하는 방식으로, ref는 current 프로퍼티를 가진 객체입니다. 

## React 컴포넌트 내에서 id가 아니라 ref를 사용하는 이유는 무엇인가요?
- id는 DOM에서 유일해야하는데, React 컴포넌트는 여러번 사용될 수 있기 때문에 id를 사용하는것이 적절하지 않습니다.
- 반면에 ref는 전역적으로 작동하지 않고 컴포넌트 내부에서만 작동하기 때문입니다.

## React에서 ref를 사용하는 방법은 무엇인가요?

### 1. 클래스형 컴포넌트에서 ref를 사용하는 방법
- React.createRef 메소드를 사용합니다.
- 변수.current로 접근할 수 있습니다
- React 16.3 버전부터 사용 가능함

```jsx
class RefSample extends Component {
  input = React.createRef();
 
  handleFocus = () => {
    this.input.current.focus();
  }

  render() {
    return (
      <div>
        <input ref={this.input} />
        <button onClick={() => this.handleFocus}>
          포커스
        </button>
      </div>
    );
  }
}

```

### 2. 함수형 컴포넌트에서 ref를 사용하는 방법
- useRef 훅을 사용해야 합니다. (함수 호출에 관계없이 state를 유지하기 때문에 훅을 써야한다.)
- 변수.current로 접근할 수 있습니다

```jsx
import { useRef } from "react";

function RefSample() {
  const input = useRef();
  const handleFocus = () => {
    input.current.focus();
  }

  return (
    <div>
      <input ref={input} />
      <button onClick={handleFocus}>
        포커스
      </button>
    </div>
  );
}
```


### 3. 콜백함수를 통해 사용하는 방법
- ref 프로퍼티에 콜백함수를 전달합니다.
```jsx
<input 
  ref={(ref) => this.input = ref}
>

```

## 실무에서 ref를 사용하는 예시로는 어떤 것이 있을까요? 
1. input 요소에 포커스 주기
2. 스크롤 박스 조작하기
3. canvas 요소에 그림 그리기

### 공식문서 (레거시)
1. 포커스, 텍스트 선택영역, 혹은 미디어의 재생을 관리할 때.
2. 애니메이션을 직접적으로 실행시킬 때.
3. 서드 파티 DOM 라이브러리를 React와 같이 사용할 때.


## ref와 state의 차이점은 무엇인가요?
1. ref는 DOM을 직접적으로 건드려야하는 때 사용하고, state는 컴포넌트의 상태를 관리할 때 사용한다.
2. ref는 current 프로퍼티를 가진 객체이고, state는 컴포넌트의 상태를 관리하는 객체이다.
3. ref는 변경 시 리렌더링이 일어나지 않는다. state는 리렌더링이 일어난다.
4. ref는 Mutable하고, state는 Immutable하다.

|ref|state|
|---|---|
|useRef(initialValue) => `{ current : initialValue}` 반환| useState(initialValue) => `[state = initialValue, setState]` 반환|
|변화 시 리렌더링이 일어나지 않는다|변화할 때마다 리렌더링이 일어난다|
|Mutable - 렌더링 프로세스 외부에서 current 값을 수정하고 업데이트할 수 있음 | Immutable - setState를 통해서 수정해 리렌더링을 대기열에 추가해야함|
|You shouldn’t read (or write) the current value during rendering. 렌더링 중에는 current 값을 읽거나 쓰지 말아야한다.| You can read state at any time. However, each render has its own snapshot of state which does not change. state를 언제든 읽어올 수 있다. 그러나, 각 렌더링은 변화되지 않은 state을 스냅샷으로 갖고 있다.|
|비제어컴포넌트|제어 컴포넌트|

- cf) ref는 자주 사용하지 않는 탈출구 라고 한다. (Refs are an “escape hatch” you won’t need often. -리액트 공식문서)


## forwardRef는 무엇인가요?
- 함수형 컴포넌트에서 부모 컴포넌트가 자식 컴포넌트의 DOM 요소에 접근하고자 할 때 사용하는 메서드이다.
- forwardRef를 통해 ref를 prop으로 받아서 원하는 element에 전달할 수 있다.
- forwardRef로 자식 컴포넌트를 감싸고 전달하고자 하는 ref를 props 다음 두번째 인자로 전달한다.)

```jsx
function App() {
  const ref = useRef(null);

  useEffect(() => {
    console.log(ref);
  }, [ref]);

  return (
    <div className="App">
      <StyledInput ref={ref} />
    </div>
  );
}

const StyledInput = forwardRef((props, ref) => {
  return <input ref={ref} />;
});
```


## 참고한 글

- [리액트 공식문서](https://react-ko.dev/learn/referencing-values-with-refs)
- [React ref 톺아보기](https://tecoble.techcourse.co.kr/post/2021-05-15-react-ref/)
- [React-forwardRef-사용법](https://dygreen.tistory.com/entry/React-forwardRef-%EC%82%AC%EC%9A%A9%EB%B2%95)