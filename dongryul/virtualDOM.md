# virtual DOM이란 무엇인가요? 
>Virtual DOM은 메모리에 존재하는 가상의 DOM 트리를 말합니다. UI의 갱신을 효율적으로 수행하기 위하여 실제 DOM의 구조를 띄며 메모리상에서 동작하는 객체입니다.

## (꼬리질문) virtual DOM의 동작 원리는 무엇인가요?
>  데이터가 변경되면, 가상 DOM의 UI가 변경되고, 실제 DOM과 비교하여 바뀐 부분만 적용합니다.

## (꼬리질문) 재조정(Reconciliation) 이란?
> UI의 가상적인 표현을 메모리에 저장하고 ReactDOM과 같은 라이브러리에 의해 실제 DOM과 동기화하는 과정을 말합니다.

## (꼬리질문) virtual DOM의 장점은 무엇인가요?
> 변경사항이 생길 때마다 전체 렌더트리를 다시 그리는 것(실제 DOM API 조작)이 아니라, 변경된 부분만 업데이트하여 성능을 향상시킬 수 있습니다.

## (꼬리질문) virtual DOM을 사용하는 것이 항상 빠를까요?
> 인터렉션이 많은 웹앱에서는 빠르지만, 정보제공만 하는 단순한 웹페이지 같은 경우에는 오히려 실제 DOM 조작보다 느릴 수 있습니다.

## (꼬리질문) key prop을 사용하는 이유는 무엇인가요?
> 어떤 자식 엘리먼트가 변경되지 않아야 할지 표시해줌으로서 렌더링 효율성을 높일 수 있기 때문입니다.

---

## 설명 자료

### virtual DOM은 실제 DOM을 추상화한 자바스크립트 객체이다.
- 실제 DOM의 속성들을 갖고 있음 (id, class, style, innerHTML 등)
- DOM API는 갖고 있지 않음(getElementById, querySelector 등)

### virtual DOM의 예시
```js
let domNode = {
  tagName: 'div',
  attributes: {
    id: 'container',
    class: 'container',
    style: 'background-color: red;'
  },
  children: [
    {
      tagName: 'h1',
      attributes: {},
      children: ['Hello, Virtual DOM!']
    }
  ]
}
```

- virtual DOM은 메모리 상에서 동작하기 때문에 빠르게 연산 가능하고 실제 렌더링되지 않기 때문에 연산 비용이 적다.


### 데이터가 변경되면, 가상 DOM의 UI가 변경되고, 실제 DOM과 비교하여 바뀐 부분만 적용한다.
![](https://miro.medium.com/v2/resize:fit:700/1*8OCCATi8_5HmWI1QpjrRNA.png)
- 비교 (Diffing) : virtual DOM이 업데이트 되면 업데이트 이전의 virtual DOM과 비교하여 정확히 어떤 virtual DOM Element가 변경되었는지 확인한다. 효율적인 알고리즘을 통해 변경된 지점을 빠르게 파악할 수 있다.
- 속성 값만 변했다면 그것만 업데이트
- element 태그나 컴포넌트가 변경되었다면 해당 element를 삭제하고 새로운 Virtual DOM element를 추가한다.
- 새로운 virtual DOM을 만들어 실제 DOM과 비교하여 바뀐 부분만 적용한다.
- Batch update: 10곳이 바뀌었다면 10번 렌더링하는 것이 아니라, 바뀐 부분을 한꺼번에 업데이트 한다.

## key Prop을 사용하는 이유
- key prop을 통해, 여러 렌더링 사이에서 어떤 자식 엘리먼트가 변경되지 않아야 할지 표시해줌으로서 렌더링 효율성을 높일 수 있다.
- 해당 key는 오로지 형제 사이에서만 유일하면 되고, 전역에서 유일할 필요는 없습니다.
- 배열의 인덱스를 key로 사용하면, 배열의 요소가 변경되어 인덱스도 변경될 경우 의도치않은 방식으로 렌더링될 수 있다. 

### 참고자료

[[10분 테코톡] 🥁 지그의 Virtual DOM](https://youtu.be/PN_WmsgbQCo?si=C-cYsH-rtqaX3cLE)

[별코딩 - React의 가상돔 (Virtual DOM)이 뭔가요? (짱 쉬움)](https://youtu.be/gc-kXt0tjTM?si=R-SWPo-JhccOnI2a)

[[React] 리액트 reconciliation(재조정) 이란? / 리액트 key
[개발자 아저씨들 힘을모아:티스토리]](https://programming119.tistory.com/240)

[리액트 레거시 공식문서 '재조정'](https://ko.legacy.reactjs.org/docs/reconciliation.html)

[[React] 리액트를 처음부터 배워보자. — 03. React 의 Update 스케줄링 과정](https://medium.com/crossplatformkorea/react-%EB%A6%AC%EC%95%A1%ED%8A%B8%EB%A5%BC-%EC%B2%98%EC%9D%8C%EB%B6%80%ED%84%B0-%EB%B0%B0%EC%9B%8C%EB%B3%B4%EC%9E%90-03-react-%EC%9D%98-reconciliation-%EA%B3%BC%EC%A0%95-2e6fb59c0c2d)