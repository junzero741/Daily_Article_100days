8월 17일 화요일

링크 : https://medium.com/humanscape-tech/%EC%9E%90%EB%B0%94%EC%8A%A4%ED%81%AC%EB%A6%BD%ED%8A%B8-%ED%81%B4%EB%A1%9C%EC%A0%80%EB%A1%9C-hooks%EA%B5%AC%ED%98%84%ED%95%98%EA%B8%B0-3ba74e11fda7


# 클로저란?
* 함수로 하여금 `private` 한 변수를 가질 수 있도록 하는 것
```javascript
function getAdd() {
  let foo = 1;
  
  return function() {
    foo = foo + 1;
    return foo;
  }
}

const add = getAdd();

console.log(add());   // 2
console.log(add());   // 3
```
* add에 할당된 `getAdd()`는 생성당시 외부스코프에 있는 변수 `foo`를 기억해, 호출될때마다 `foo`의 값에 접근이 가능하다.
* `foo`의 값은 외부에서는 접근이 불가능하다. 따라서 `private`한 변수를 가질 수 있게 되는 것


# 클로저로 useState 구현하기
```javascript

function useState(iniVal) {
  let _val = initVal;
  const state = () => _val;
  const setState = newVal => {
    _val = newVal;
  };
  
  return [state, setState];
}

const [count, setCount] = useState(1);

console.log(count()); // 1

setCount(2);
console.log(count()); // 2

```
* useState 함수에서 리턴한 `state` 값을 통해서만 `_val` 변수에 접근가능하게 하고, 
* useState 함수에서 리턴한 `setState` 값을 통해서만 `_val` 변수를 변경하도록 하는 것.


# React 구현하기
```javascript

const React = (function() {
  let _val;
  
  function useState(initVal) {
    const state = _val || initVal;
    const setState = newVal => {
      _val = newVal;
    };

    return [state, setState];
  }
  
  function render(Component) {
    const C = Component();
    C.render();
    return C;
  }
  
  return { useState, render };

})();

function Component() {
    const [count, setCount] = React.useState(1);
    
    return {
      render: () => console.log(count),
      click: () => setCount(count+1),
    }

const App = React.render(Component);
App.click();  // 1

```
* React 함수의 메소드를 이용해서만 `_val` 변수에 접근이 가능하도록 React 함수 내부에 useState와 render 함수를 넣어놓았다.

# state가 하나 이상인 경우


```javascript
const React = (function() {
  // 변경
  let hooks = [];
  let idx = 0;
  
  function useState(initVal) {
  // 변경
    const _idx = idx;
    const state = hooks[idx] || initVal;
    const setState = newVal => {
      // 변경
      hooks[_idx] = newVal;
    };
    idx++;
    return [state, setState];
  }
 
  function render(Component) {
    idx = 0;
    const C = Component();
    C.render();
    return C;
  }
  
  return { useState, render };
})();

function Component() {
  const [count, setCount] = React.useState(1);
  const [text, setText] = React.useState('apple');
  
  return {
    render: () => console.log({ count, text }),
    click: () => setCount(count + 1),
    type: (word) => setText(word),
  }
}

var App = React.render(Component); // {count: 1, text: "apple")
App.click(); 
var App = React.render(Component); // {count: 2, text: "apple"}
App.type('pear');
var App = React.render(Component); // {count: 2, text: "pear"}
```
* setState 안의 index값이 useState에 의해 변하지 않도록 freeze 시켜주었다.
* React Hooks 규칙 중, Hooks 를 최상위에서만 호출해야 하는 이유가 위 코드에 들어있다.
* render될 때마다 특정 index에 state가 순서대로 할당되므로 최상위에서 불러야 하는 것.
