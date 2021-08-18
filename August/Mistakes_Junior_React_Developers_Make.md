8월 18일 수요일

링크 : https://frontend-digest.com/mistakes-junior-react-developers-make-c546b1af187d

# 서문
* 이 글은 많은 주니어 React 개발자를 곁에서 지켜봐온 어느 개발자가 그들이 하는 대표적인 실수들을 언급한다.
* 그리고 어떻게 이를 피할 수 있는지 서술하고 있다.


# 문제1. DOM 직접 조작
* jQuery를 써본 개발자들에게서 특히 자주 발견되는 실수이다.
* 아래 코드를 먼저 보자.
```javascript
import React from "react";
export class Menu extends React.Component {
  componentDidMount() {
    if (window.innerWidth < 500) {
      const menu = document.querySelector(".menu");
      menu.classList.add("mobile");
    }
  }
  render() {
    return <div className="menu">{this.props.children}</div>;
  }
}
```
## 문제가 뭘까?
* 리액트에서는, 돔 엘리먼트에 접근해 클래스를 추가하면 좋지 않다.
* 대신 리액트 컴포넌트 내부의 상태를 이용해 컴포넌트에 클래스를 추가하는 것이 장려된다.

## 문제되는 이유는?
* 웹앱을 만드는 데에 있어서 가장 중요한 것은 앱의 상태를 관리하는 것이다.
* 앱이 커질수록, 상태는 점점 커지고 복잡해질 것이다.
* 만약 앱이 DOM내부의 상태와 뒤섞이고, 리액트 내부의 상태와 섞인다면 앱은 점점 디버깅과 테스트하기가 점점 어려워질 것.

## 해결방법
```javascript
import React from "react";
export class Test extends React.Component {
  state = {
    isMobile: false
  };
  componentDidMount() {
    if (window.innerWidth < 500) {
      this.setState({
        isMobile: true
      });
    }
  }
  render() {
    return (
      <div className={this.state.isMobile ? "mobile" : ""}> // 상태에 따라 바뀌는 className
        {this.props.children}
      </div>
    );
  }
}
```
* state를 통해 className을 업데이트했고, `document.querySelector`는 더이상 쓰이지 않는다!


# 문제2. 이벤트 리스너 클린업을 까먹는다
* 유저가 스페이스바를 눌렀을 때를 예로 들어보자.
```javascript
import React from "react";
export class CaptureSpace extends React.Component {
  componentDidMount() {
    document.body.addEventListener("keydown", event => {
        if (event.keyCode === 32) {
          // do something when user hits spacebar
        }
    });
  }
render() {
    return (
       //
    );
  }
}
```
* 이벤트 리스너를 추가했지만, `componentDidMount` 의 호출이 끝나는 시점에 리스너를 제거하지 않았다.
* 이는 메모리 누수로 이어질 위험이 있고, 문제를 디버깅하기도 어렵게 만든다.
* 컴포넌트가 언마운트 되기 전에 이벤트 리스너를 삭제해주는 것이 좋다.
* 아래 코드를 보자.
```javascript
import React from "react";

export class CaptureSpace extends React.Component {
  componentDidMount() {
    document.body.addEventListener("keydown", this.captureSpace);
  }
componentWillUnmount() {
    document.body.removeEventListener("keydown", this.captureSpace);
  }
captureSpace = event => {
    if (event.keyCode === 32) {
      // do something when user hits spacebar
    }
  };
render() {
    return (
       //
    );
  }
}
```


# 문제3. 테스트를 (충분히)하지 않는다
* 주니어 개발자들이 테스트를 특히 무서워 하는 이유는 컴포넌트나 앱이 복잡해질수록, 테스트 코드를 작성하는 일 자체에 압도되기 때문이다.
* 나는 주니어 개발자들이 순수 함수에 대한 유닛 테스트를 작성하는건 종종 보지만, 전체 컴포넌트에 대한 통합 테스트를 하는건 보기 쉽지 않았다.
* 아래 코드를 보자. 이메일과 패스워드를 입력받아 API를 호출하고, 긍정적인 응답이 오면 다른 페이지로 리다이렉트 시켜주는 로직이다.
```javascript
import React from "react";
import { LoginForm } from "./Login";
import axios from "axios";
import { render } from "@testing-library/react";
import userEvent from "@testing-library/user-event";

jest.mock("axios");  // axios 함수를 모킹한다.

// 로그인 폼을 테스트한다고 명시
describe("LoginForm", () => {
  // 무엇을 테스트할지 명시한다.
  describe("when a user types in a correct email and password", () => {
    // 테스트를 통해 기대하는 결과를 명시
    it("should redirect to home page", async () => {
  
      // 로그인 폼 소환
      const rendered = render(<LoginForm />);
      
      // 로그인 폼에 Email 란에 유저가 Email을 작성했다고 가정
      const emailInput = rendered.getByLabelText("Email");
      await userEvent.type(emailInput, "john@gmail.com");

      // 로그인 폼에 password 란에 유저가 password를 작성했다고 가정
      const passwordInput = rendered.getByLabelText("Password");
      await userEvent.type(passwordInput, "1234");

      // Email, Password 모두 제대로 입력했다면 해당 링크로 이동한다고 가정
      delete window.location;
      window.location = { replace: jest.fn() };

      // data 를 post 했다고 가정
      const data = { data: { token: "fake-token" } };
      axios.post.mockImplementationOnce(() => Promise.resolve(data));

      // 유저가 Submit 버튼을 클릭했다고 가정
      userEvent.click(rendered.getByText("Submit"));

      // 윈도우 URL이 "/home" 이 나오면 테스트 통과!
      expect(window.location.replace).toHaveBeenCalledWith("/home");
    });
  });
});


```

# 문제4. 웹팩을 이해하지 않는다
* 아주 적은 수의 주니어 개발자들만이 웹팩 사용법을 이해한다.
* 대부분의 주니어 개발자들은 주어진 코드 베이스에서만 개발하고, 일단 코드가 돌아가면 만족해버린다.
* 그들이 작성하는 CSS와 ES6 가 어떻게 형성되어 유저의 브라우저에 번들링되는지 깊게 알아보지 않는다.


## 해결책은?
* 모든 리액트 개발자들은 그들만의 보일러플레이트를 만들어볼 것을 추천한다.
* CRA나 NextJS에 의존하지 않고, 어떻게 모던 자바스크립트 빌드 툴이 작동하는지 이해하는 시간이 필요하다.
* 이는 스스로의 작업물에 대해 좀 더 잘 이해해주고, 빌드될때 나타나는 문제들을 디버깅하기 편하게 해줄 것이다!
