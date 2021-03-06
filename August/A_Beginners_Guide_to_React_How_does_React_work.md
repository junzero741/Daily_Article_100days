8월 14일 토요일

# 리액트란 무엇인가?
* 선언적 코드를 이용해 예측가능하고 효율적인 방법으로 UI를 그려주는 라이브러리.

# 선언적(declarative) 코드란?
 * 어떤 일(what) 을 해야 할지만 명시하고, 어떻게(how) 할 지는 관심을 두지 않는 것.
 * 즉 선언형 코드는 최종 결과만 기술한다.
 * 가볍고, 이해하기 쉬우며, 리팩토링하기도 쉽고 버그도 적다.

# 리액트에서 컴포넌트란?
* 우리는 컴포넌트를 만들기 위해 선언적인 코드를 작성한다.
* 컴포넌트란 각자 독립적으로 작동하면서, 블럭들로 앱을 분리할 수 있게 해주는, 재사용가능한 UI 라고 할 수 있다.
* 컴포넌트는 임의의 데이터 인풋 (prop) 을 받아 화면에 렌더링해줘야 할 리액트 엘리먼트를 리턴한다.

# 앱의 상태란 ?
* 복잡한 UI를 만들기 위해서는 컴포넌트들을 논리적인 방법으로 정렬해야 하는데, 이때 필요한게 상태이다.
* 상태는 한마디로 어느 시점에 앱이 갖는 스냅샷이라고 할 수 있다.
* 선언적인 UI에서, 개발자들은 이벤트 발생에 따른 UI의 변화를 담당하진 않는다.
* 명령적인 UI였다면 div의 display 여부를 토글하는 거에 대해 걱정했겠지만 말이다.
* 개발자는 그저 단순히 앱의 상태에 대해서만 걱정하면 된다. 
* 앱은 그저 가만히 있고, 앱의 상태가 변하면서 렌더링에 관여하는 것이다.
* 우리는 한번에 하나의 컴포넌트의 상태만 그리면 된다.

# 리액트에선 컴포넌트를 어떻게 관리할까?
* 리액트의 컴포넌트는 상태와 prop으로 만들어져있다.
* 이 두 재료가 의미하는 것은, 컴포넌트들을 계층적인 구조로 관리할 수 있다는 것이다.
* 이러한 컴포넌트들은 단방향 데이터 흐름을 가지고 있다(prop을 통해)
* 우리는 이를 컴포넌트 트리라 부르고, 이 트리는 하나의 상태에게 하나의 컴포넌트에 대한 책임을 갖도록 한다.
* 컴포넌트 트리는 상태를 헷갈리지 않고 복잡한 UI를 그릴 수 있게 해준다.
