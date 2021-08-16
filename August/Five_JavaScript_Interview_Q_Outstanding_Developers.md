8월 16일 월요일

링크 : https://javascript.plainenglish.io/5-javascript-interview-questions-to-identify-outstanding-developers-859a71c3d7f


# 서문
* 자바스크립트는 어디에나 있다. (웹, 앱, IoT, CLI, RESTful API, ...)
* 동시에 JS의 표준 형식인 ECMAScript로 인해 JS는 짧은 주기로 발전해왔다.
* 따라서 현대 개발 커뮤니티엔 JS 개발자가 매우 많으므로, 이중 뛰어난 JS 개발자를 뽑는 방법이 중요해졌다.
* 본문에서는 이를위한 인터뷰 질문들을 소개한다. 
* 참고로 최신 문법이 아니라, JS 지식에 대한 깊이에 관련된 것들이다.


# Q1. 호이스팅
* **질문** : 함수가 아직 선언되지 않았음에도 다음 코드가 작동하는 이유는?
```javascript
sayHello();

function sayHello() {
  console.log("Hello World!");
}

// Hello World!
```
* **답변** : 함수 선언은 호이스팅되기 때문에, `sayHello` 함수는 코드 실행 이전, 스코프의 최상단으로 이동된다.
따라서 JS 런타임이 실행될 때 `sayHello` 함수가 이미 선언되었다는 것을 알고 있는 것이다.



# Q2. 비동기 재귀
* **질문** : 재귀 Promise로 팩토리얼 함수를 실행해보라. 재귀 함수 호출은 자체는 상관 없지만, 팩토리얼 함수 안에서는 프로미스를 재귀적으로 반환해야 한다.
* **답변** : 문제에서 비동기 솔루션을 원하므로, 팩토리얼 함수안에서 Promise를 반환해야 한다. 따라서, 값을 직접 반환하는 대신 `resolve` 콜백을 써서 각 재귀 호출을 만족해야한다. 인풋 파라미터(`n`)에 의존해서 promise 체이닝을 해야한다. 코드를 보자.
```javascript
function factorialPromise(n) {
    return new Promise((resolve, reject) => {
        if (n <= 1) {
            resolve(1);
        } else {
            resolve(
                factorialPromise(n - 1)
                .then((prevFact) => (prevFact * n))
            );
        }
    });
}

factorialPromise(5)
    .then((ans) => {
        console.log(`fact(5) = ${ans}`);
    });
    
// fact(5) = 120
```


# Q3. async/await 문제
* **질문** : 함수에 `asnyc` 키워드를 더하면 어떻게 될까? 또, 글로벌 스코프에서 어떻게 비동기 함수를 blocking call 처럼 만들 수 있을까?
* **답변** : `async` 키워드를 쓰면, 자바스크립트 런타임은 특정 함수의 코드를 Promise로 감싼다. 함수 내부에서 반환되는 것은 자동으로 resolve처리가 된다. 따라서, async 함수는 언제나 Promise object를 반환한다.
await 키워드를 사용하면 비동기 콜을 동기로 만들 수 있다. async 함수 내에서 `await` 키워드를 사용한다. 만약 우리가 글로벌 스코프에서 `await` 스코프를 써야한다면, IIFE를 만들수 있겠다.
```javascript
async function getTen() {
  return 10;
}

(async () => {
  let ten = await getTen();
  console.log(ten);

})()

// 10
```


# Q4. 단순 closure
* **질문** : 다음 코드가 어떻게 작동하는지 말해보라. 여기서 사용된 JS 개념은 무엇인가?
```javascript
function nAdder(x) {
  return (y) => (x + y);
}

let fiveAdder = nAdder(5);
```
* **답변** : `nAdder` 함수는 주어진 값과 부모 함수의 스코프의 값을 합을 반환하는 함수를 반환한다. 우리가 부모 함수를 호출할 때(nAdder), 자식 함수가 사용하는 첫번째 변수(x)에 할당될 값을 전달해줄 수 있다. 또, 자식 함수에서 쓰이는 두 번째 값도 전달해줄 수 있다. 이 개념은 클로저이다.


# Q5. magic 함수
* **질문** : 인자의 개수를 유동적으로 받을 수 있는 함수를 작성하고, 접근법을 설명해보라.
* **답변** : 이전 JS 런타임에서는 `arguments` 오브젝트를 사용할 수 있다. `arguments` 오브젝트는 특정 함수에 전달된 인자들의 배열을 담고 있다. 새로운 JS 런타임(ES6) 에서는 rest parameter 문법을 사용할 수 있다.
```javascript
function showValuesOld() {
  for(let i of arguments)
    console.log(i);
}

function showValuesNew(...values) {
  for(let i of values)
   console.log(i);
}
```


# 맺으며
* JS 라이브러리나 프레임워크로 개발 시간을 줄이려는 회사들이 많다.
* 그러나 몇몇 회사들은 JS의 작동원리를 깊이 파고든다.
* 저자는 JS의 핵심 컨셉들(i.e Promise, async/await, closures, context, hoisting, ...) 을 이해하는 개발자들이 특정 라이브러리에 경험 많은 개발자들보다 더 뛰어나다는 생각을 갖고 있다.
