8월 27일 금요일

링크 : https://blog.bitsrc.io/understanding-execution-context-and-execution-stack-in-javascript-1c9ea8642dd0




# JS의 실행 컨텍스트와 실행 스택 이해하기
> 자바스크립트가 어떻게 내부적으로 작동하는지 알아보자.

## 서문
* 실행 컨텍스트와 실행 스택에 대해 이해하는 것은 호이스팅, 스코프, 클로저를 이해하기 위한 초석이라고 할 수 있다.

## 실행 컨텍스트란?
* 자바스크립트 코드가 평가되고 실행되는 환경에 대한 추상적인 개념이라고 할 수 있다.
* JS 에서 실행되는 모든 코드는 실행 컨텍스트 내부에서 실행된다고 말할 수 있다.
* 이러한 실행 컨텍스트는 총 3가지 타입이 있다.

## 실행 컨텍스트 type_1  : 글로벌 실행 컨텍스트
* 가장 기본이 되는 실행 컨텍스트.
* 어느 함수에도 속하지 않은 코드는 글로벌 실행 컨텍스트에 있다고 볼 수 있다.
* 브라우저를 사용할 경우엔 글로벌 오브젝트 (window 오브젝트)를 생성한다.
* this 의 값은 글로벌 오브젝트와 동일하다.
* 하나의 프로그램에는 오직 하나의 글로벌 실행 컨텍스트만 존재할 수 있다.


## 실행 컨텍스트 type_2  : 함수 실행 컨텍스트
* 함수가 호출될 때마다, 해당 함수는 자신만의 새로운 실행 컨텍스트가 생성된다.
* 함수 실행 컨텍스트에는 개수 제한이 없다.

## 실행 컨텍스트 type_3 :Eval 함수 실행 컨텍스트
* eval 함수 안에서 실행되는 코드는 자신만의 실행 컨텍스트를 갖는다.


## 실행 스택

* 코드가 실행되는 동안 생성되는 모든 실행 컨텍스트를 저장하는 데에 쓰인다.
* JS 엔진이 스크립트를 만나 코드가 실행되면, 우선 글로벌 실행 컨텍스트를 생성한 뒤 현재의 실행 스택에 넣는다.
* 엔진이 함수 선언을 만나면, 해당 함수를 위한 새로운 실행 컨텍스트를 생성하고 생성한 실행 컨텍스트를 실행 스택 맨 위에 push 한다.
* 엔진은 실행 스택 맨 위에 있는 실행 컨텍스트의 함수를 실행한다. 
* 해당 함수의 실행이 끝나면 실행 스택에서 실행 컨텍스트가 pop되고, 다음 실행 컨텍스트로 
* 예시를 보자.
```javascript
let a = 'Hello World!';
function first() {
  console.log('Inside first function');
  second();
  console.log('Again inside first function');
}
function second() {
  console.log('Inside second function');
}
first();
console.log('Inside Global Execution Context');

```

![image](https://user-images.githubusercontent.com/71166372/131156651-2b08254d-1d58-4f55-be89-cc9d2ff97487.png)

* 위 코드가 브라우저에 로드되면, 자바스크립트 엔진은 글로벌 실행 컨텍스트를 생성해 실행 스택에 push한다.
* `first()` 를 만나면, 자바스크립트 엔진은 해당 함수를 위한 새로운 실행 컨텍스트를 만들어 현재의 실행 스택에 push한다.
* `second()` 가 `first()` 내부에서 실행될때, 자바스크립트 엔진은 second를 위한 새로운 실행 컨텍스트를 생성해 현재의 실행 스택에 push한다.
* `second()`가 끝나면, second의 실행 컨텍스트는 현재의 실행 스택에서 pop 되고, 아래에 있는 `first()` 실행 컨텍스트로 이동한다.
* `first()` 가 끝나면, first의 실행 컨텍스트 역시 스택에서 제거되고, 글로벌 실행 컨텍스트로 이동한다.
* 모든 코드가 실행되면, 자바스크립트 엔진은 글로벌 실행 컨텍스트를 현재의 스택에서 제거한다.


## 어떻게 실행 컨텍스트가 생성될까?
* JS 엔진이 어떤 원리로 실행 컨텍스트를 생성하는지 알아보자.
* 실행 컨텍스트는 두가지 단계를 거쳐 생성된다.


## 실행 컨텍스트가 생성되는 단계_1 : 생성 단계
* 실행 컨텍스트는 생성 단계에서 생성된다.
* 이 단계에서는 다음과 같은 일들이 일어난다.
* **LexicalEnvironment** 컴포넌트가 생성된다.
* **VariableEnvironment** 컴포넌트가 생성된다.
* 따라서 실행 컨텍스트는 컨셉적으로 아래와 같이 표현될 수 있다.
```javascript
ExecutionContext = {
  LexicalEnvironment = <ref. to LexicalEnvironment in memory>,
  VariableEnvironment = <ref. to VariableEnvironment in  memory>,
}
```

## Lexical Environment란?
* 식별자-변수 맵 을 갖고 있는 구조이다.
* 여기서 식별자는 변수나 함수의 이름이다.
* 변수는 실제 객체(함수 객체와 배열 객체 또는 원시 값을 포함하는)의 참조이다.
* 아래 코드를 보자.
```javascript
var a = 20;
var b = 40;

function foo () {
   console.log('bar');
}
```
* 위 코드의 lexical environment는 아래와 같다.
```javascript
lexicalEnvrionment = {
  a : 20,
  b : 40,
  foo : <ref. to foo function>
}
```
* 각각의 Lexical Environment는 세가지 컴포넌트를 갖고 있다.
* 1. Environment Record
* 2. 외부 environment의 참조
* 3. this 바인딩


## Envrionment Record란?
* lexical environment 내부에 변수와 함수 선언이 저장되는 장소.
* environment record에는 두가지 타입이 있다.

## Declarative envrionment record
* 이름에서 알 수 있듯, 변수와 함수의 선언을 저장한다.
* 함수 코드를 위한 lexical environment에는 declarative environment record가 포함되어 있다.

## Object environment record
* 글로벌 코드를 위한 lexical environment는 객체 environment record가 포함되어 있다.
* 변수와 함수 선언과 별개로, object environment record는 글로벌 바인딩 오브젝트 (브라우저의 window)를 저장한다.
* 따라서 각 바인딩 객체의 프로퍼티는(브라우저의 경우, 브라우저에 의해 제공되는 윈도우 오브젝트의 프로퍼티와 메소드를 포함하고 있는) 새로운 엔트리가 record에 생성된다.


> 함수 코드의 environment record는 `arguments` 오브젝트도 포함하고 있다. 
> `arguments` 오브젝트는 인덱스와 매개변수들의 매핑, 그리고 함수에 전달된 매개변수의 길이(갯수)를 포함하고 있다.
> 예시로, 아래 함수를 보자.

```javascript
function foo (a,b) {
  var c = a + b;
}

// argument object
Arguments: {0: 2, 1: 3, length: 2}
```

## 외부 environment 의 참조
* 외부 lexical environment로 접근할 수 있다는 뜻이다.
* 이는 자바스크립트 엔진이, 현재의 lexical environment에서 a 라는 변수를 찾을 수 없다면, 외부 환경에서 a라는 변수를 검색할 수 있다는 뜻.


## this 바인딩
* 글로벌 실행 컨텍스트에서, `this`의 값은 글로벌 오브젝트를 가리킨다 (브라우저의 경우, 윈도우 오브젝트)
* 함수 실행 컨텍스트에서는, `this` 의 값은 어떻게 함수가 호출되었느냐에 따라 달라진다.
* 만약 함수가 오브젝트 참조에 의해 호출되었다면, `this`의 값은 그 오브젝트에 종속된다.
* 그렇지 않다면, `this` 의 값은 글로벌 오브젝트나 `undefined` (strict mode의 경우)로 정해진다.
* 아래 예시를 보자.


```javascript
const person = {
 name: 'peter',
  birthYear: 1994,
  calcAge: function() {
    console.log(2018 - this.birthYear);
  }
}

person.calcAge(); 
// 'this' 는 person을 가리킨다. calcAge 함수가 person 오브젝트에 의해 호출되었기 때문이다.
const calculateAge = person.calcAge;
calculateAge();
// 'this' 는 글로벌 윈도우 오브젝트를 가리킨다. 아무런 오브젝트가 주어지지 않았기 때문.

```

* 수도 코드로 lexical environment를 나타내자면 아래와 같다.

```
GlobalExectionContext = {
  LexicalEnvironment: {
    EnvironmentRecord: {
      Type: "Object",
      // Identifier bindings go here
    }
    outer: <null>,
    this: <global object>
  }
}
FunctionExectionContext = {
  LexicalEnvironment: {
    EnvironmentRecord: {
      Type: "Declarative",
      // Identifier bindings go here
    }
    outer: <Global or outer function environment reference>,
    this: <depends on how function is called>
  }
}
```


## Variable Environment
* variable environment 또한 lexical environment라고 할 수 있다.
* 따라서 variable environment는 lexical environment 의 모든 프로퍼티와 컴포넌트를 갖고 있다.
* ES6에서, variable environment와 lexical environment의 차이는 후자의 경우 함수 선언과 변수 (let, const) 바인딩을 저장한다는 점이고,
* variable environment는 `var` 바인딩만 저장하는데 쓰인다.


## 실행 단계
* 모든 변수 할당이 끝나고, 코드가 마침내 실행되는 단계이다.

```javascript

let a = 20;
const b = 30;
var c;

function multiply(e,f) {
  var g = 20;
  return e * f * g;
}

c = multiply(20,30);
```
* 위 코드가 실행될때, 자바스크립트 엔진은 글로벌 코드를 실행하기 위해 글로벌 실행 컨텍스트를 생성한다.
* 따라서 글로벌 실행 컨텍스트는 **생성 단계**에서 아래와 같이 생겼다.

```
GlobalExectionContext = {
  LexicalEnvironment: {
    EnvironmentRecord: {
      Type: "Object",
      // Identifier bindings go here
      a: < uninitialized >,
      b: < uninitialized >,
      multiply: < func >
    }
    outer: <null>,
    ThisBinding: <Global Object>
  },
  VariableEnvironment: {
    EnvironmentRecord: {
      Type: "Object",
      // Identifier bindings go here
      c: undefined,
    }
    outer: <null>,
    ThisBinding: <Global Object>
  }
}
```
* 실행 단계에서 변수 할당이 마무리되므로, **실행 단계**에서의 글로벌 실행 컨텍스트는 아래와 같다.

```
GlobalExectionContext = {
LexicalEnvironment: {
    EnvironmentRecord: {
      Type: "Object",
      // Identifier bindings go here
      a: 20,
      b: 30,
      multiply: < func >
    }
    outer: <null>,
    ThisBinding: <Global Object>
  },
VariableEnvironment: {
    EnvironmentRecord: {
      Type: "Object",
      // Identifier bindings go here
      c: undefined,
    }
    outer: <null>,
    ThisBinding: <Global Object>
  }
}
```

* 함수 `multiply(20, 30)` 실행문을 만나면, 함수 코드를 실행하기 위해 새로운 함수 실행 컨텍스트가 생성된다.
* 따라서 **생성 단계**에서의 함수 실행 컨텍스트는 아래와 같다.

```
FunctionExectionContext = {
LexicalEnvironment: {
    EnvironmentRecord: {
      Type: "Declarative",
      // Identifier bindings go here
      Arguments: {0: 20, 1: 30, length: 2},
    },
    outer: <GlobalLexicalEnvironment>,
    ThisBinding: <Global Object or undefined>,
  },
VariableEnvironment: {
    EnvironmentRecord: {
      Type: "Declarative",
      // Identifier bindings go here
      g: undefined // 여기 주목
    },
    outer: <GlobalLexicalEnvironment>,
    ThisBinding: <Global Object or undefined>
  }
}
```

* 생성 단계 이후, 실행 컨텍스트는 실행 단계로 넘어간다.
* 이는 함수 내부에서 변수에의 값 할당이 끝났다는 의미이다.
* 따라서 **실행 단계** 에서의 실행컨텍스트는 아래와 같다.


```
FunctionExectionContext = {
LexicalEnvironment: {
    EnvironmentRecord: {
      Type: "Declarative",
      // Identifier bindings go here
      Arguments: {0: 20, 1: 30, length: 2},
    },
    outer: <GlobalLexicalEnvironment>,
    ThisBinding: <Global Object or undefined>,
  },
VariableEnvironment: {
    EnvironmentRecord: {
      Type: "Declarative",
      // Identifier bindings go here
      g: 20 // g 에 값이 할당되었다!
    },
    outer: <GlobalLexicalEnvironment>,
    ThisBinding: <Global Object or undefined>
  }
```

* 함수가 끝나고, 반환된 값은 `c` 변수에 저장된다.
* 따라서 글로벌 lexical environment 가 업데이트된다.
* 이후 글로벌 코드가 끝나고 프로그램이 종료된다.

> `let` 과 `const`는 생성 단계에서 아무런 값도 갖지 않는(초기화되지 않은) 반면,
> `var` 는 `undefined`로 초기화된다는 점에 주목할 필요가 있다.
>  또 함수 선언은 중괄호 블록까지 environment에 저장된다.

* 이것이 바로 `var`로 선언한 변수는 선언 이전에도 접근할 수 있는 이유이다. (`undefined`로 뜨긴 하지만)
* 반대로 `let`과 `const`는 참조 에러가 뜨는 것과도 연관있다.
* 이것이 바로 **호이스팅** 이다.



