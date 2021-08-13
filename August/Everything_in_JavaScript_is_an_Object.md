8월 13일 금요일

링크 : https://dev.to/d4nyll/not-everything-in-javascript-is-an-object


# JS 는 OOP 언어인가 FP 언어인가?
* OOP로도, FP로도 작동할 수 있다고함

# JS의 데이터 타입
* 원시형: Boolean, null, undefined, number, string, symbol(ES6) 
* object 타입 : key와 value로 이루어짐

## 함수와 배열은 모두 Object type이다
```javascript
// Primitive types
true instanceof Object; // false
null instanceof Object; // false
undefined instanceof Object; // false
0 instanceof Object; // false
'bar' instanceof Object; // false

// Non-primitive types
const foo = function () {}
foo instanceof Object; // true
```

## 원시 타입에는 메소드가 없다는 사실을 기억하라.
* 때문에 원시타입은 immutable하다. mutate 할 메소드가 없기 때문.


# 객체타입은 참조로, 원시타입은 값으로 저장된다.
```javascript
"dog" === "dog"; // true
14 === 14; // true

{} === {}; // false    {} 의 메모리 주소와 {} 의 메모리 주소는 다르다.
[] === []; // false
(function () {}) === (function () {}); // false
```


# 함수는 Object 타입의 한 종류다.
* 함수만이 갖고있는 특별한 프로퍼티가 있는데, `constructor`와 `call`이다.
* 객체의 한 종류이기 때문에 일급 시민이라고 할 수 있다.
* 다른 객체들처럼 함수의 인자로 전달되거나 함수가 반환할 수 있다.


# 생성자 함수
* 똑같이 구현할 오브젝트들이 있으면, 생성자 함수에 그 로직을 넣어 그러한 오브젝트들을 생성할 수 있다.
* 생성자함수는 다른 함수랑 똑같다. 어떤 함수든 앞에 `new` 키워드를 붙여주면 생성자함수가 된다.
* 화살표함수는 안된다. (역자)


# String, Number, Boolean, Function 함수들
* String 함수는 글로벌 함수로, 인자로 넣은 값을 원시값(스트링)으로 변환을 시도한다.


# Auto-Boxing
* "pet".constructor === String 은 `true`다.
* 원시값이 어떻게 메소드를 가지는걸까?
* 이 프로세스를 오토박싱이라 부른다.
* 유저가 원시 타입의 프로퍼티(ex: length)나 메소드를 부르려 할때 원시 타입을 래퍼 오브젝트로 오토박싱한다.
* 그 래퍼객체의 length 프로퍼티에 접근하고, 바로 버린다.
* 이 과정은 원래의 원시 타입에 아무런 영향을 끼치지 않고 일어난다.

# 요약
* 자바스크립트의 모든 것이 객체는 아니다.
* JS에는 6개의 원시 타입들이 있다(Number, String, Boolean, null, undefined, Symbol)
* 원시 타입이 아닌 것은 모두 객체다
* 함수는 객체의 특별한 타입일 뿐이다
* 함수는 새로운 오브젝트를 만드는 데에 쓰일 수 있다 (생성자함수)
* 문자열, 불리언, 숫자는 원시타입이면서 오브젝트일수도 있다.
* 특정한 원시타입들 (string, number, boolean)은 오브젝트처럼 행동하는데, 자바스크립트의 특징 중 하나인 오토박싱 때문이다.
