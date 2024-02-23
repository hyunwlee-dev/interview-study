# (면접) FLUX 아키텍처란 무엇인가요?

사용자의 행위 `action`은 `dispatcher`에 의해 통제됩니다. `dispatcher`가 `store`를 업데이트하고 변경된 `store`에 대한 `view`를 리렌더링합니다. `view`에서는 `store`에 직접 접근하지 않으며, `dispatcher`로 다시 액션을 보내고 `store`를 업데이트한 뒤, 다시 `view`를 리렌더링하는 단방향적 구조를 가집니다.  

`FLUX` 패턴은 이러한 단방향적인 데이터 흐름 구조를 통해 어떤 액션이 디스패처에 의해 어떤 결과를 낳고 변화되는지 명확히 파악하고 알아볼 수 있습니다.



---



## Overview

페이스북은 왜 Flux 패턴이 필요했는지 가장 잘 알려져 있는 것은 알림(notification) 버그이다.  

![](https://bestalign.github.io/static/79ef489156c8d6979a03014b20ed1d6f/0a47e/01.png)

로그인 했을 때 화면 위의 메시지 아이콘에 알림이 떠 있지만, 그 메시지 클릭해서 들어가 보면 아무 메시지가 없던 적이 있었다. 이 버그를 고치고 얼마 동안은 괜찮았지만 곧 다시 나타났다.   

그래서 Facebook은 시스템을 더욱 예측 가능하게 만들어서 문제점을 없애길 원했다.   

### MVC 패턴의 근본적인 문제점

![](https://velog.velcdn.com/images/andy0011/post/4a1f159f-6972-4028-8a18-9a383cf5e44d/image.png)

근본적인 문제점은 데이터가 애플리케이션을 흐르는 방법에 있었다.  

MVC는 Model, View, Controller의 약자로, 데이터를 저장하고 관리하는 Model, 사용자에게 보여지는 View, 그리고 이 둘을 중개하는 Controller로 구성된다. 사용자가 View를 통해 데이터를 입력하면 이는 Controller를 통해 Model을 업데이트하고, Model의 변경은 View에 반영된다. 하지만 애플리케이션이 커지고 복잡해지면, 이러한 양방향 데이터 흐름이 예측하기 어려워진다.  

### 단방향 데이터 흐름의 해결책

그래서 Facebook은 다른 종류의 아키텍처를 시도하기로 결정했다. 데이터는 단방향으로만 흐르고, 새로운 데이터를 넣으면 처음부터 흐름이 다시 시작된다. 이러한 단방향적인 데이터 흐름 구조를 통해 어떤 액션이 dispatcher에 의해 어떤 결과를 낳고 변화되는지 명확히 파악하고 알아볼 수 있게 되었다.

![flux-diagram](https://haruair.github.io/flux/img/flux-simple-f8-diagram-with-client-action-1300w.png)

---

### Action

- 데이터를 변경하는 행위로서 Dispatcher에게 전달되는 객체를 말한다.

### Action Creator

- 모든 변경사항과 사용자와의 상호작용이 거쳐가야 하는 action의 생성을 담당하고 있다. 언제든 애플리케이션의 상태를 변경하거나 view를 업데이트하고 싶다면 action을 생성해야만 한다.
- action creater는 새로 발생한 action 타입(type)과 새로운 데이터(payload)를 묶어 dispatcher에게 전달한다.

```json
{
  type: 'ADD_TODO',
  text: '블로그 쓰기'
}
```

### Dispatcher

- 모든 데이터는 중앙 허브인 dispatcher를 통해 흐른다.

- 기본적으로 콜백(callback)이 등록되어 있는 곳이다. action을 보낼 필요가 있는 모든 store를 가지고 있고 action creator로부터 action이 넘어오면 여러 store에 action을 보낸다.
- 동기적으로 처리 되기 때문에 하나를 다른 것보다 먼저 업데이트해야 한다면, waitFor()을 사용하여 dispatcher가 적절히 처리하도록 할 수 있다. 또한 action type과 관계없이 등록된 모든 store로 action을 보내기 때문에 store가 특정 action만 구독하지 않고 모든 action을 받고 처리를 결정할 수 있다.

### Store

- 애플리케이션 내의 모든 상태와 그와 관련된 로직을 가지고 있다.
- 모든 상태 변경은 반드시 store에 의해서 결정되어야한다. store에 직접 상태변경 요청을 전달할 수 없다. 무조건 action creator/dispatcher 파이프라인을 거쳐서 액션을 보내야만 한다. Dispatcher로부터 전달받은 Action에 따라 데이터를 업데이트한다.
- store의 내부에서는 보통 switch statement를 사용해서 받은 모든 액션 중 처리할 액션과 무시할 액션을 결정하고 상태를 변경하게 된다.
- 상태 변경 완료 후 컨트롤러 뷰에 상태가 변경했다는 것을 알려준다.

### View

- 리액트 컴포넌트로 생각하면 된다.

### Controller View

- store와 view 사이의 중간관리자같은 역할을 한다.
- 상태가 변경되었을 때 store가 그 사실을 controller-view에게 알려주면, controller-view는 자신의 아래에 있는 모든 view에게 새로운 상태를 넘겨준다.

---

> ref
>
> - https://github.com/junh0328/prepare_frontend_interview/blob/main/react.md#flux%EC%97%90-%EB%8C%80%ED%95%B4%EC%84%9C-%EC%95%84%EB%82%98%EC%9A%94
> - https://bestalign.github.io/translation/cartoon-guide-to-flux/
> - https://haruair.github.io/flux/docs/overview.html
