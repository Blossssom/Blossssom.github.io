---

layout: post

title: 22 / 08 / 05 My TIL [Redux-toolkit]

subtitle: SEB TIL

categories: coding

tags: [React]

---
# redux-toolkit

<aside>
💡 기존에 사용하던 Redux를 사용하면서 느낀것 처럼 약간의 문제점이 있다. 

1. 저장소의 복잡성
2. 다수의 패키지와 의존성
3. 코드의 가독성과 양

하나의 액션을 생성하는 과정에서 Action의 타입 정의, 함수 생성, Reducer 정의 와 같이 긴 작업이 필요하지만 `redux-toolkit` 을 사용하면 기존의 단점을 보완할 수 있는 기능을 제공한다.

redux를 보다 간편하게 사용할 수 있는 `redux-toolkit` 을 사용해보고 어떤 점에서 편한지 느껴보자.

</aside>

## Setting

<aside>
💡 우선 redux-toolkit을 설치하고 프로젝트에 세팅하자.

</aside>

```bash
npm install @reduxjs/toolkit
yarn add @reduxjs/toolkit
```

프로젝트에 사용하는 매니지먼트 프로그램에 따라 위와 같은 명령어를 사용해 툴킷을 설치할 수 있다.

### package.json

<aside>
💡 redux-toolkit은 redux를 기본적으로 포함하고 있어 기존에 redux를 사용해 package.json의 redux를 지워도 무방하다.

</aside>

사실 세팅이라고 말하기 힘들정도로 간단하다. `redux-toolkit` 은 redux의 몇가지 특징을 단순화하여 편의를 제공하기 때문에 기존의 redux와 구성, 동작이 동일하게 이뤄진다.

매번 새로운 라이브러리를 만날때 마다 새롭겠지만 라이브러리의 아이덴티티를 먼저 파악하고 가면 조금이나마 학습이 용이한 느낌이다.

## Store

<aside>
💡 redux-toolkit을 사용하기 위해 당연히 저장소를 만들어야 한다. 하나씩 차근차근 학습해보자.

</aside>

### createAction

```jsx
const modalAction = createAction('modalToggle');

let action = modalAction();
// {type: 'modalToggle'}

action = modalAction(4);
// {type: 'modalAction', payload: 4}
```

- 기존의 Redux와 달리 한번의 과정으로 action의 생성이 가능하며 `createAction` 의 인자로 타입을 넣을 수 있고 payload 또한 액션함수의 호출과 함께 전달할 수 있다.

### createReducer

```jsx
const counterReducer = createReducer(0, {
	increment: (state, action) => state + action.payload,
	decrement: (state, action) => state - action.payload
});
```

- 기존의 if, switch과정이 없이 `createReducer` 의 첫 인자로 initialState, 두 번째로 case를 가질 수 있다.

### createAction + createReducer

```jsx
const increment = createAction('increment');
const decrement = createAction('decrement');

const counterReducer = createReducer(0, {
  [increment]: (state, action) => state + action.payload,
  [decrement.type]: (state, action) => state - action.payload
});
```

### createSlice = action + reducer

```jsx
initState = [];

createSlice({
	name: 'reducerName',
	initialState: initState,
	reducers: {
		action01(state, action) {
			// logic
		},
		action02(state, action) {
			// logic
		}
	} 
});
```

- 위에서 설명한 `createAction, createReducer` 를 합쳐 `createSlice` 로 사용할 수 있다.
- name은 액션의 경로를 나타내고 initialState는 초기 state를 나타낸다. 기존과 달리 reducers에 선언된 로직 함수가 dispatch되면 바로 state를 가지고 액션을 처리한다.

### configureStore

```jsx
import { configureStore } from '@reduxjs/toolkit';

export const store = configureStore({
	reducer: {
		counter: counterReducer,
		modal: modalReducer
	}
});
```

- 기존과 달리 `combineReducers` 를 사용해 리듀서를 묶을 필요없이 reducer 객체를 통해 다수의 reducer를 제공할 수 있다.
- default로 redux devtool을 제공한다.

## 비교하기

- 기존

```jsx
//액션타입 
const increase = "INCREASE";
const decrease = "DECREASE";
const increaseFive = "INCREASE_FIVE";

//액션생성함수
export const increaseAction = () => {
  return { type: increase };
};

export const decreaseAction = () => {
  return { type: decrease };
};

export const increaseOtherAction = (number) => {
  return { type: increaseFive, data: number };
};

//초기 상태값
const initialState = {
  number: 30,
};

//리듀서
export const numberReducer = (state = initialState, action) => {
  switch (action.type) {
    case increase:
      return {
        ...state,
        number: state.number + 1,
      };

    case decrease:
      return (state = {
        ...state,
        number: state.number - 1,
      });

    case increaseFive:
      return (state = {
        ...state,
        number: state.number + action.data,
      });

    default:
      return state; 
  }
};
```

- toolkit

```jsx
import { createSlice, PayloadAction } from '@reduxjs/toolkit';
import { AppThunk, RootState } from '../../app/store';

export const counterSlice = createSlice({
  name: 'counter',
  initialState: {
    value: 0,
  }; 
  reducers: {
    increment: state => {
      state.value += 1;
    },
    decrement: state => {
      state.value -= 1;
    },
    incrementByAmount: (state, action: PayloadAction<number>) => {
      state.value += action.payload;
    },
  },
});

export const { increment, decrement, incrementByAmount } = counterSlice.actions;
export default counterSlice.reducer;
```

---
<br><br>
## 마무리

