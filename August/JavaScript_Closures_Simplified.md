8월 19일 목요일

링크 : https://medium.com/swlh/javascript-closures-simplified-262d935466ec


# 서문
* JS에서 클로저는 많은 개발자들이 주목하고 있으며, 앞으로도 중요하게 쓰일 컨셉 중 하나다.
* 이 포스트에서는 클로저가 무엇인지, 이해하기 쉽게끔 현실 세계를 예로 들어 설명한다.
* 저자는 **클로저는 곧 유산(Legacy)** 라고 받아들이면 이해하기 쉽다고 한다.
* 본문을 읽고나면 왜 그런지 알게 될 것이다.


# 클로저란 무엇인가?
* 클로저는 내부 함수가 외부 함수의 변수에 접근할 수 있는(스코프 체인이라고도 함) 자바스크립트의 특징을 말한다.

## 클로저는 세가지 스코프 체인을 갖는다.
1. 변수가 정의된 위치 `{}` 에 접근할 수 있다.
2. 외부 함수에 접근할 수 있다.
3. 전역 변수에도 접근할 수 있다.


# 코드로 이해해보자
## 단순 클로저
```javascript
function parent() {
    var house = 'WhiteHouse';
   
    function child() {   
        var car = 'Tesla'; 
        console.log('I have:', house, car);
    }
   
    return child;
}
var legacy = parent();
console.log(typeof(legacy)); //type of legacy is function
legacy(); I have: WhiteHouse Tesla
```
* `house` 변수를 갖고 있는 외부함수 `parent`는 `child` 함수를 반환하고 있다.
* `car` 변수를 갖고 있는 내부함수 `child` 는 함수 내부에서 `parent` 함수의 변수인 `house`에 접근하고 있다.
* `parent` 함수를 호출하는 부분에서, `parent()`의 결과는 함수타입인 `legacy` 변수에 저장된다.

## legacy() 가 실행되면 무슨 일이 벌어질까?
1. `car` 변수가 생성되고, `Tesla` 라는 값이 할당된다.
2. JS 는 `console.log("I have: ", house, car)` 를 실행하려 한다.
3. **`car` 변수는 방금 만들어졌으니 당연히 존재하지만, `house` 변수는 더이상 존재하지 않는다. 왜냐하면 `house`는 `parent` 함수의 일부였으므로, `house`는 `parent()`가 실행되는 동안에만 존재할 수 있다.**
4. **다시말해, `legacy()` 가 실행된 시점에서  `parent()` 함수는 진작에 실행이 끝났으므로 `parent`의 내부함수인 `house`는 더이상 존재하지 않는다는 말이다.**
5. 그렇다면 위 코드는 어떻게 실현 가능했을까?



## parent 함수의 변수가 child 함수에게 남겨준 유산
* `child` 함수는 JS 의 클로저 덕분에 외부함수의 변수에 접근할 수 있다.
* 다시말해, `parent` 함수는 자신이 실행되는 동안에,  `child` 함수에 본인의 스코프 체인을 **유산(legacy)** 으로 넘겨주는 것이다.
* 때문에 `child` 함수는 보존된 legacy 변수(`house`)에 접근할 수 있는 것이다.


> **클로저는 외부 함수가 갖고 있는 변수에 대한 참조를 저장한다**
>
> 실제 값을 저장하고 있는 것이 아니다. 
> 따라서 클로저가 호출되기 이전에, 외부 함수의 변수 값이 변할 수 있다. 
> 이러한 특징은 창의적인 코드를 짜는데 종종 이용되곤 한다.



# 유산으로서의 클로저
* 클로저가 왜 유산인지에 대해 좀 더 자세하게 알아보기 위해, 또다른 중첩 함수 예시를 보자.
```javascript
function grandParent() {
    var house = 'GreenHouse';
   
    function parent() {   
        var car = 'Tesla';
        house = 'YellowHouse';
        function child() {   
            var scooter = 'Vespa';
            console.log('I have:', house, car, scooter);
        }
        
        return child;
    }
   
    return parent;
}
var legacyGenX = grandParent();
console.log(typeof(legacyGenX)); //legacyGenX is of type function
var legacyGenY = legacyGenX();
console.log(typeof(legacyGenY)); //legacyGenY is of type function
legacyGenY(); // I have: YellowHouse Tesla Vespa
```
* 지금까지 설명한 바를 토대로 위 코드를 이해해보자면, 가장 안쪽에 있는 `child()` 함수는 `house`와 `car`에 접근할 수 있다.
* `grandParent()`가 자신의 `house`를 `parent()` 에게 물려줬고, `parent`는  `house`의 색깔을 초록색에서 노란색으로 바꿨다.
* `parent`는 자신이 물려받은 `house`와, 자신이 만든 `car`를 다시 `child` 에게 물려줬다.
* ![image](https://user-images.githubusercontent.com/71166372/130099166-d9665b48-af90-4754-a42b-c82473491fda.png)



# 결론
* 클로저는 여러 JS 개념 중 한번에 이해하기 쉽지 않은 편에 속하지만, 한번 이해하면 세상이 달라보일 것이다.
* 저자가 클로저를 기억하는 방법은, **클로저는 유산(legacy)** 라는 것이다.
* 함수가 생성되고 다른 함수로부터 반환될때, 자신이 갖고 있던 모든 변수들을 자식 함수에 넘겨주기 때문이다.
