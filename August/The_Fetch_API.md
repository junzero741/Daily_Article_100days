8월 24일 화요일

링크 : https://medium.com/beginners-guide-to-mobile-web-development/the-fetch-api-2c962591f5c



# The Fetch API
> XMLHttpRequest의 모던 대체재



## AJAX 도입 이전
* Asynchronous JavaScript and XML 의 줄임말.
* AJAX 도입 이전에는 페이지 일부분을 업데이트 하기 위해 브라우저는 서버에 웹페이지 전체를 요청하곤 했다.
* 서버가 요청을 받으면, 페이지를 만들어서 브라우저에 다시 응답을 보내주었다.
* 이 때문에 페이지의 아주 작은 부분만 변하더라도 페이지 전체가 다시 로딩되어야만 했다.

## AJAX 도입 이후
* AJAX 는 기존의 전통적인 페이지 업데이트 방식을 바꿨다.
* AJAX로 인해 웹 앱들은 **비동기적으로** 서버에 데이터를 보내거나, 받을 수 있게 되었다.

## XMLHttpRequest
* 2006년, 월드와이드웹 컨소시움(W3C)은 `XMLHttpRequest` 오브젝트를 발표했다.
* 이는 서버로부터 데이터를 비동기적으로 받기 위해 사용되었다.
* `XMLHttpRequest`는 `XML` 데이터를 `HTTP` 프로토콜 위에서 fetch 하기위해 사용되었다.
* 그러나 오늘날에는 `HTTP` 프로토콜 외의 다른 프로토콜과 함께 사용되며, `XML` 데이터 폼 뿐만 아니라 `JSON`, `HTML`, 일반 텍스트까지 fetch할 수 있게 되었다.
* 예시 코드를 보자.

```javascript

function success() {
  const data = JSON.parse(this.responseText);
  console.log(data);
 }
  
function error(err) {
  console.log('Error Occurred : `, err);  
 }
 
const xhr = new XMLHttpRequest();
xhr.onload = success;
xhr.onerror = error;
xhr.open('GET', 'https://api.github.com/users/swapnilbangare');
xhr.send();
```

* success 와 error를 처리하기 위해 `onload` 와 `onerror` 두 리스너가 `xhr` 변수에 부착되었다.
* `open()` 과 `send()` 를 호출하면, 서버로부터의 응답이 `responseText` 변수에 저장되고, `JSON.parse()` 함수에 의해 자바스크립트 객체로 변환된다.

> 지난 몇년간, 이 `XMLHttpRequest` 는 `XML` 외의 다른 데이터를 요청하기 위해서 사용되었는데, 이는 수많은 초보 개발자들로 하여금 JS에서의 비동기 요청 방법을 배우기 어렵게 했다.
> 그래서, 비동기 요청을 날릴 심플하고 클린한 API가 필요하게 되었고, 이게 **Fetch API** 의 등장 배경이다.


## Fetch
* fetch는 `XMLHttpRequest` 와 비슷하게 네트워크 요청을 가능하게 해준다.
* 다른 점은, `fetch` 가 `XMLHttpRequest`와 달리 **Promise를 사용함으로써 콜백을 피하게 해준다는 점이다!**

## fetch 의 인터페이스
* `fetch()` : 데이터를 fetch 하기 위해 사용되는 메소드
* `Headers` : 응답/요청 헤더를 나타낸다. 헤더를 쿼리에 포함시켜 결과에 따라 다른 액션을 취할 수 있게 해준다.
* `Request` : 데이터 요청을 나타낸다.
* `Response` : 요청에 따른 데이터 응답을 나타낸다.

## fetch()로 요청을 날려보자
* `fetch()` 함수는 글로벌 `window` 객체에서 사용 가능하다.
* fetch 요청을 날릴 경로를 꼭 인자로 넣어주어야 한다.
* fetch 요청이 실패하든 성공하든 `Promise` 객체를 반환한다.
* 요청이 성공하면 `then()` 함수가 `Response` 객체를 받으며, 실패하면 `catch()` 함수가 `error` 객체를 받는다.

```javascript
  fetch('https://api.github.com/users/swapnilbangare')
    .then(function(response) {
      console.log(response);
     })
     
     .catch(function(err) {
       console.log(`something went wrong!`, err);
     });
```

## Response(응답) 객체
* 위의 코드는 어느 깃허브 유저의 데이터를 받아온다.
* **Promise가 resolved 될 때 우리는 `Reponse` 객체를 응답으로 받는다. **
* 그러나 `response`를 바로 콘솔에 찍으려 하면, 우리가 원하는 데이터가 바로 나오진 않는다. `Response` 객체에는 응답 그 자체에 대한 정보가 들어있기 때문이다.
* 실제 데이터를 얻으려면 우리는 응답의 `body`에 접근해야 한다.
* 위 코드에서 깃허브 API는 `JSON` 을 반환하므로, 응답 객체에는 `.json()` 메소드가 포함되어 있을 것이다. 따라서 `response`를 `.json()` 메소드에 파라미터로 넣어서 실행해야 한다.
* 이 때,  `Response` 객체에 사용하는 `.json()` 메소드 역시 `Promise` 객체를 반환하므로, 또 다른 `then()` 체이닝을 해줘야한다.

```javascript
fetch('https://api.github.com/users/swapnilbangare')
    .then(function (response) {
        return response.json();
    })
    .then(function (data) {
        console.log(data);
    })
    .catch(function (err) {
        console.log("Something went wrong!", err);
    });
```


## Header 객체
* `Headers` 인터페이스는 `Headers()` 생성자 함수를 통해 나만의 헤더 객체를 만들수 있게 해준다. 헤더 객체는 이름-값 짝의 콜렉션이다.

```javascript
const headers = new Headers();
headers.append('Content-Type', 'text/json');
headers.apeend('X-Custom-Header', 'SomeValue');

// 오브젝트 리터럴을 생성자 함수에 넘겨 똑같은 결과를 얻을 수 있다.

const headers = new Headers({
  'Content-Type': 'text/json',
  'X-Custom-Header': 'SomeValue'
});

```


## fetch() 함수에 옵션 적용하기
* `fetch()` 함수는 옵셔널한 두번째 파라미터를 받을 수 있는데, 이를 `init` 오브젝트라고 한다.
* `init` 오브젝트로 리퀘스트를 커스터마이징 할 수 있다.

```javascript

  let reqHeader = new Headers();
  reqHeader.append('Content-type`, `text/json`);
  let initObject = {
    method: 'GET', headers: reqHeader,
  };
  
  fetch('https://api.github.com/users/swapnilbangare', initObject)
    .then(function (response) {
        return response.json();
    })
    .then(function (data) {
        console.log(data);
    })
    .catch(function (err) {
        console.log("Something went wrong!", err);
    });

```


## Reqeust 객체
* `Request` 객체는 요청 리소스를 나타낸다.
* `fetch()` 함수에 URL을 넘기는 대신, `Request()` 생성자 함수로 요청 객체를 생성할 수 있다.
* 이 요청 객체를 `fetch()` 함수에 인자로 넣어 커스터마이즈된 요청을 날릴 수 있다.

```javascript
let reqHeader = new Headers();
reqHeader.append('Content-Type`, 'text/json');

let initObject = {
  method: 'GET', headers: reqHeader,
}

const userRequest = new Request('https://api.github.com/users/swapnilbangare', initObject);

fetch(userRequest)
    .then(function (response) {
        return response.json();
    })
    .then(function (data) {
        console.log(data);
    })
    .catch(function (err) {
        console.log("Something went wrong!", err);
    });
```


## 결론
* `XMLHttpRequest` 는 우리가 오늘날 쓰는 것들을 위해 만들어지지 않았음을 알 수 있었다. 또, API가 너무 너저분하기도 하다.
* 대신, `Fetch` API는 비동기 리퀘스트를 날리고 응답을 관리하기 훨씬 쉽게 해준다.
* `fetch`는 모던 자바스크립트 속성인 `promise`를 통해 우리가 더 나은 API를 만들 수 있도록 제공한다!
