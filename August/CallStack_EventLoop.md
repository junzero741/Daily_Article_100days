8월 12일 목요일

링크 : https://medium.com/@gaurav.pandvia/understanding-javascript-function-executions-tasks-event-loop-call-stack-more-part-1-5683dea1f5ec

# Understanding Javascript Function Executions — Call Stack, Event Loop , Tasks & more

## 루트 레벨에서 JS가 어떻게 작동하는지 아는 것이 중요하다.
* 어느 프레임워크를 사용하든 기본 토대 지식을 알아야 제대로 써먹을 수 있기 때문.

## JS는 싱글스레드로 작동한다.
* 한번에 하나의 태스크(or 코드) 만 수행할 수 있음.

## 콜스택
* 함수의 호출을 기록하는 데이터 구조.
* 함수를 호출하면 콜스택에 뭔가가 쌓인다.
* 해당 함수로부터 리턴을 받으면, 콜스택이 pop 된다.

## 힙
* 오브젝트들은 힙이라는 곳에 할당된다.
* 가장 비구조화된 메모리 지역.
* 모든 변수, 오브젝트 할당이 이곳에서 일어난다.

## 큐
* JS 런타임은 메시지 큐를 가지고 있다.
* 실행될 콜백 함수들과 관련된 큐들임.
* 스택이 비었을때 큐로부터 메시지가 나와서 처리된다.

## 이벤트 루프
* 프로그램의 실행을 막는 block script들이 있다(i.e while(true)console.log())
* 비동기 AJAX 통신 덕분에 네트워크 요청을 하고, 답변을 받기 전에 화면이 그려질 수 있는 것
* 이벤트루프의 역할은 콜스택과 task queue를 둘 다 지켜보면서 스택이 비었을때 큐의 첫번째 원소를 스택에 밀어놓는 것!
