23번째 글

# Redux



## InitialState

* 관리하고자 하는 상태의 초기값.
* 식료품점의 매대에 진열된 여러 상품을 생각해보자.

```javascript
const initialBodegaState = {
    insideBodega : false,
    choppedCheese : 10,
    catMood : "Neutral",
};
```

> Redux --initalState



<br/>



## Actions/Dispatches

* action (행동) 에 의해 initalState 가 변한다.
* action이 발생했다는 것을 dispatch 되었다고 한다.

```javascript
const ENTER = {
    type : "ENER_BODEGA",
};

const BUYFOOD = {
    type : "BUY_CHOPPED_CHEESE",
    buy: 1,
};

const PETCAT = {
    type : "PET_BODEGA_CAT",
};

const LEAVE = {
    type : "LEAVE_BODEGA",
};
```

> Redux -- Actions



<br />



## Reducers

* Reducer 는 액션을 해석한다.
* state ( 처음에는 initialState) 와 action 을 인자로 받는다.
* 어떤 액션이냐에 따라서 상태를 어떻게 업데이트 할 지를 정할 수 있다.

```javascript
const bodegaReducer = (state = initialBodegaState, action) => {
    switch(action.type) {
        case 'ENTER_BODEGA' :
            return {
                ...state,
                insideBodega: true,
            };
        case 'BUY_CHOPPED_CHEESE':
            return {
                ...state,
                choppedCheese: Math.max(0,state.choppedCheese - action.buy),
            };
        case 'PET_BODEGA_CAT':
            return {
                ...state,
                catMood: "Happy",
            }
        case 'LEAVE_BODEGA':
            return {
                ...state,
                insideBodega: false,
            }
        default:
            return state;
    };
};
```

> Redux -- reducer



<br />



## Store

* action 에 의해 state가 변하고, 어떤 action이냐에 따라 state 를 어떻게 변화시킬지를 정할 reducer도 만들었다.
* store 는 이들에 의해 변한 state를 저장하는 역할을 한다.
* store 를 만들기 위해선 reducer와 initialState가 필요하다.

```javascript
const store = Redux.createStore(bodegaReducer, initialBodegaState);
```

> Redux -- Store





* `store.getState()` 를 통해 앱 내 어디에서든 상태를 불러올 수 있다.

```javascript
// store.getState() 를 실행하면 반환되는 현재 상태.

// before entering - no actions called
{
    insideBodega: false,
    choppedCheese: 10,
    catMood:"Neutral",
}
    
// after entering - ENTER_BODEGA dispatched once
{
    insideBodega: true,
    choppedCheese: 10,
    catMood:"Neutral",
}
    
// after buying two sandwiches - BUY_CHOPPED_CHEESE dispatched twice
{
    insideBodega: false,
    choppedCheese: 8,
    catMood:"Neutral",
}
    
// after petting the bodega cat - PET_BODEGA_CAT dispatched once
{
    insideBodega: true,
    choppedCheese: 8,
    catMood: "Happy",
}
    
// after leaving - LEAVE_BODEGA dispatched once
{
    insideBodega: false,
    choppedCheese: 8,
    catMood: "Happy",
}
```

> Redux -- using store.getState()



<br />



## Subscribe

* store 에 subscribe 를 걸어 store 에 변화가 있을 때 notify를 받을 수 있다.

```javascript
const stateListener = () => {
    const currentBodegaState = store.getState();
    if (currentBodegaState.catMood === 'Neutral') {
        console.log('I should pet the bodega cat!');
    } else {
        console.log('What a happy bodega cat!');
    };
};

store.subscribe(stateListener);
```

> Redux -- Subscriptions



<br />



## REFERENCE

https://medium.com/@the.asantiagojr/how-does-redux-work-b1cce46d4fa6

