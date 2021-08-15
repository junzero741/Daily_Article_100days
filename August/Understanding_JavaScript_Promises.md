8월 15일 일요일

# Promise 이해하기
* 내가 아이라고 상상해보자. 엄마가 다음주에 새 휴대폰을 사주겠다고 약속했다면 어떨까?
* 엄마가 진짜 폰을 사줄지 나는 알 수 없다. 사줄 수도, 안사줄 수도 있다.
* 이게 Promise다. Promise는 세 가지 상태를 갖는다.

# Promise의 3가지 상태
* Pending: 새 폰을 갖게 될지 알 수 없는 상태
* Fulfilled: 엄마가 새 폰을 사준 상태
* Rejected: 엄마가 폰을 사주지 않는 상태

# Promise를 만들어보자
```javascript
const isMomHappy = false;

const willIGetNewPhone = new Promise(
    function (resolve, reject) {
        if (isMomHappy) {
            const phone = {
                brand: 'Samsung',
                color: 'black'
            };
            resolve(phone); // fulfilled
        } else {
            const reason = new Error('mom is not happy');
            reject(reason); // reject
        }

    }
);
```

# Promise를 사용해보자
```javascript
const willIGetNewPhone = ... // continue from part 1

// call our promise
const askMom = function () {
    willIGetNewPhone
        .then(function (fulfilled) {
            // yay, you got a new phone
            console.log(fulfilled);
             // output: { brand: 'Samsung', color: 'black' }
        })
        .catch(function (error) {
            // oops, mom didn't buy it
            console.log(error.message);
             // output: 'mom is not happy'
        });
};

askMom();
```


# Promise는 체이닝이 가능하다
* 엄마가 휴대폰을 사줬을 경우, 친구에게 자랑을 하는 상황을 가정해보자.
```javascript
// 2nd promise
const showOff = function (phone) {
    return new Promise(
        function (resolve, reject) {
            const message = 'Hey friend, I have a new ' +
                phone.color + ' ' + phone.brand + ' phone';

            resolve(message);
        }
    );
};

// shorten 버전

var showOff = function (phone) {
    const message = 'Hey friend, I have a new ' +
                phone.color + ' ' + phone.brand + ' phone';

    return Promise.resolve(message);
};
```

* 체이닝을 해보자. `showOff`는 `willIGetNewPhone` 이 실행된 이후에만 실행될 수 있다.
```javascript
// call our promise
const askMom = function () {
    willIGetNewPhone
    .then(showOff) // chain it here
    .then(function (fulfilled) {
            console.log(fulfilled);
         // output: 'Hey friend, I have a new black Samsung phone.'
        })
        .catch(function (error) {
            // oops, mom don't buy it
            console.log(error.message);
         // output: 'mom is not happy'
        });
};
```

# Promise는 비동기적으로 작동한다
```javascript
const askMom = function () {
    console.log('before asking Mom'); // log before
    willIGetNewPhone
        .then(showOff)
        .then(function (fulfilled) {
            console.log(fulfilled);
        })
        .catch(function (error) {
            console.log(error.message);
        });
    console.log('after asking mom'); // log after
}


// before asking Mom
// after asking mom
// Hey friend, I have a new black Samsung phone.
```

* 새 폰을 받을수 있을지, 없을지를 알 수 있을 때까지 우리는 아무것도 하지 않는게 아니다.
* Promise가 완료될 때까지 기다릴 필요가 있을 때는 `then`을 사용한다.


# ES6 버전의 Promise

```javascript
//_ ES6: Full example_

const isMomHappy = true;

// Promise
const willIGetNewPhone = new Promise(
    (resolve, reject) => { // fat arrow
        if (isMomHappy) {
            const phone = {
                brand: 'Samsung',
                color: 'black'
            };
            resolve(phone);
        } else {
            const reason = new Error('mom is not happy');
            reject(reason);
        }

    }
);

// 2nd promise
const showOff = function (phone) {
    const message = 'Hey friend, I have a new ' +
                phone.color + ' ' + phone.brand + ' phone';
    return Promise.resolve(message);
};

// call our promise
const askMom = function () {
    willIGetNewPhone
        .then(showOff)
        .then(fulfilled => console.log(fulfilled)) // fat arrow
        .catch(error => console.log(error.message)); // fat arrow
};

askMom();
```



# ES7 버전의 Promise는 Async/Await를 사용한다.

```javascript
// ES7: Full example
const isMomHappy = true;

// Promise
const willIGetNewPhone = new Promise(
    (resolve, reject) => {
        if (isMomHappy) {
            const phone = {
                brand: 'Samsung',
                color: 'black'
            };
            resolve(phone);
        } else {
            const reason = new Error('mom is not happy');
            reject(reason);
        }

    }
);

// 2nd promise
async function showOff(phone) {
    return new Promise(
        (resolve, reject) => {
            var message = 'Hey friend, I have a new ' +
                phone.color + ' ' + phone.brand + ' phone';

            resolve(message);
        }
    );
};

// call our promise in ES7 async await style
async function askMom() {
    try {
        console.log('before asking Mom');

        let phone = await willIGetNewPhone;
        let message = await showOff(phone);

        console.log(message);
        console.log('after asking mom');
    }
    catch (error) {
        console.log(error.message);
    }
}

// async await it here too
(async () => {
    await askMom();
})();
```

# Promise 이전에는 어땠을까?


* 그냥 두 숫자를 더해주는 일반 함수
```javascript
// add two numbers normally

function add (num1, num2) {
    return num1 + num2;
}

const result = add(1, 2); // you get result = 3 immediately
```

* 두 숫자를 더해주는 비동기 함수

```javascript
// add two numbers remotely

// get the result by calling an API
const result = getAddResultFromServer('http://www.example.com?num1=1&num2=2');
// you get result  = "undefined"
```


# 후기

* Promise 에 이해가 가지 않는 부분이 있어서 찾아 본 아티클인데,  
* 게더타운에서 동기들이 설명을 잘해줘서 아티클을 제대로 보기 전에 궁금증이 해결되어버렸다.
