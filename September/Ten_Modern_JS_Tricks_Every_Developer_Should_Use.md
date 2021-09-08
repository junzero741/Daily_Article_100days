링크 : https://betterprogramming.pub/10-modern-javascript-tricks-every-developer-should-use-377857311d79


# 모든 개발자가 써야할 10가지 JS 트릭들


## 1. 오브젝트에 프로퍼티를 조건부로 추가하기
```javascript
const condition = true;
const person = {
  id: 1,
  name: 'John Doe',
  ...(condition && { age: 16 }),
};
```

## 2. 오브젝트에 프로퍼티 존재하는지 검사하기
```javascript
const person = { name: 'John Doe', salary: 1000 };
console.log('salary' in person); // returns true
console.log('age' in person); // returns false
```

## 3. 오브젝트 프로퍼티 이름 동적으로 부여하기
```javascript
const dynamic = 'flavour';
var item = {
  name: 'Biscuit',
  [dynamic]: 'Chocolate'
}
console.log(item); // { name: 'Biscuit', flavour: 'Chocolate' }

const keyName = 'name';
console.log(item[keyName]); // returns 'Biscuit'
```


## 4. 동적 키로 오브젝트 구조 분해 할당하기
```javascript
const person = { id: 1, name: 'John Doe' };
const { name: personName } = person;
console.log(personName); // returns 'John Doe'


const templates = {
  'hello': 'Hello there',
  'bye': 'Good bye'
};
const templateName = 'bye';
const { [templateName]: template } = templates;
console.log(template) // returns 'Good bye'
```

## 5. 널 병합 연산자
```javascript
const foo = null ?? 'Hello';
console.log(foo); // returns 'Hello'
const bar = 'Not null' ?? 'Hello';
console.log(bar); // returns 'Not null'
const baz = 0 ?? 'Hello';
console.log(baz); // returns 0

const cannotBeZero = 0 || 5;
console.log(cannotBeZero); // returns 5
const canBeZero = 0 ?? 5;
console.log(canBeZero); // returns 0
```

## 6. 옵셔널 체이닝
```javascript
const book = { id:1, title: 'Title', author: null };
// normally, you would do this
console.log(book.author.age) // throws error
console.log(book.author && book.author.age); // returns null (no error)
// with optional chaining
console.log(book.author?.age); // returns undefined
// or deep optional chaining
console.log(book.author?.address?.city); // returns undefined


const person = {
  firstName: 'Haseeb',
  lastName: 'Anwar',
  printName: function () {
    return `${this.firstName} ${this.lastName}`;
  },
};
console.log(person.printName()); // returns 'Haseeb Anwar'
console.log(persone.doesNotExist?.()); // returns undefined
```

## 7. !! 연산자로 불리언 변환
```javascript
const greeting = 'Hello there!';
console.log(!!greeting) // returns true
const noGreeting = '';
console.log(!!noGreeting); // returns false
```

## 8. 문자와 정수 변환
```javascript
const stringNumer = '123';
console.log(+stringNumer); // returns integer 123
console.log(typeof +stringNumer); // returns 'number'

const myString = 25 + '';
console.log(myString); // returns '25'
console.log(typeof myString); // returns 'string'
```

## 9. 배열에서 Falsy 값 검사
```javascript
const myArray = [null, false, 'Hello', undefined, 0];
// filter falsy values
const filtered = myArray.filter(Boolean);
console.log(filtered); // returns ['Hello']
// check if at least one value is truthy
const anyTruthy = myArray.some(Boolean);
console.log(anyTruthy); // returns true
// check if all values are truthy
const allTruthy = myArray.every(Boolean);
console.log(allTruthy); // returns false

myArray.filter(val => Boolean(val));
myArray.filter(Boolean);
```

## 10. n차원 배열 flat 하기
```javascript
const myArray = [{ id: 1 }, [{ id: 2 }], [{ id: 3 }]];
const flattedArray = myArray.flat(); 
// returns [ { id: 1 }, { id: 2 }, { id: 3 } ]

const arr = [0, 1, 2, [[[3, 4]]]];
console.log(arr.flat(2)); // returns [0, 1, 2, [3,4]]
```
