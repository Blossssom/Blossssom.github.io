---

layout: post

title: 22 / 07 / 29 My TIL [Redux basic]

subtitle: SEB TIL

categories: coding

tags: [React]

---
<aside>
♦️ react를 사용하다 보면 각 컴포넌트는 마치  족보처럼 부모, 자식과 같은 1촌 사이에서만 props의 전달이 바로 이뤄질 수 있어 불편함을 느낄 수 있다. 

props를 전달하기 위해 해당 컴포넌트에서는 사용하지 않지만 그저 전달을 위해서 props를 받아와야하는 경우가 빈번한데 매우 비효율적이고 컴포넌트의 구조가 조금이라도 바뀌면 에러로 이어지는 사태가 발생할 것이다. 

이러한 불편함을 겪어 봤으니 이제 redux를 사용해 문제를 해결해보자.

</aside>

 

![Untitled](/post-img/redus-basic1.png)

<aside>
♦️ redux의 구조는 위와 같다. component wide, app wide 상태의 경우 store에서 관리하며 각 action의 요청에 따라 상태를 전달하는 구조이다. 

개인적으로 서버로부터 데이터를 받아들이는 구조와 비슷하게 느껴진다.

</aside>

 

## store

```jsx
const initialState = { counter: 0, showCounter: true };
// 이 처럼 초기 값을 따로 빼내어 사용할 수도 있다.
const counterReducer = (state = { initialState }, action) => {
    if(action.type === 'increment') {
        return {
            counter: state.counter + 1,
            showCounter: state.showCounter,
        };
        // redux는 기본상태에 변화된 것을 합치지 않고 return 값을 판단하기 때문에 초기에 지정한 showCounter도 지정해준다.
        // 즉, 기존 값과 병합되지 않고 기존 state를 덮어쓴다.
        // 위 코드에서 showCounter를 지정하지 않고 내보낸다면 showCounter 프로퍼티는 undefined로 반환될 것이다.
        // 또한 state.counter++ 과 같이 기존 값에 대한 변경은 절대 사용해선 안된다. 객체는 참조 값이라는 것을 항상 생각하자.
    }

    if(action.type === 'increase') {
        return {
            counter: state.counter + action.amount,
            showCounter: state.showCounter,
        }
    }

    if(action.type === 'decrement') {
        return {
            counter: state.counter - 1,
            showCounter: state.showCounter,
        };
    }

    if(action.type === 'toggle') {
        return {
            state: state.counter,
            showCounter: !state.showCounter,
        };
    }

    return state;
};
```

<aside>
♦️ 위 코드는 action과 payload를 받아 store의 상태를 변경하는 `reducer` 를 구현한 코드이다.

중요한 점은 상태의 원본은 절대 변형해서는 안되며, 기존 state와 병합되지 않기 때문에 변환하지 않은 상태 프로퍼티 또한 같이 전달해줘야 한다는 점이다.
state++ 과 같은 변형은 문제를 야기한다.

또한 `reducer` 는 항상 동일한 입력에 동일한 출력을 반환하는 순수함수라는 점을 기억하자.

</aside>

## method

```jsx
import { useSelector, useDispatch } from 'react-redux';
// useStore: store에 바로접근
// useSelector: 자동으로 store 상태의 일부를 선택 가능
// useDispatch: action을 저장소에 보내기위한 hook
```

<aside>
♦️ redux의 store(저장소)와 app을 연결하기 위해 사용하는 대표적인 method는 위와 같다.

`useSelector`는 useState를 사용할 때와 같이 저장소의 상태의 일부를 선택해 가져오는 역할을 한다.

`useDispatch` 는 앱에서 요청하는 action을 저장소의 reducer에 전달하기 위한 hook이다.

</aside>

```jsx
const show = useSelector(state => state.showCounter);

const dispatch = useDispatch();

const incrementHandler = () => {
    dispatch({ type: 'increment' });
    // action의 type은 저장소(store)에 지정된 identifier 중 하나여야 한다.
  };

  const increaseHandler = () => {
    dispatch({ type: 'increase', amount: 5 });
    // 전달하는 action의 프로퍼티(payload)를 추가해 사용할 수 있다.
  };
```

<aside>
♦️ component에서 상태에 접근하기 위해 위와 같은 코드를 작성하였다.

show는 `useSelector` 로 저장소의 상태를 반환받은 상태이므로 컴포넌트 내에서 일반적인 state와 같이 사용할 수 있다.

`dispatch` 는 return에 따라 위와 같이 혹은 함수를 호출한 반환값을 전달할 수 있는데 중요한 점은 type의 value가 `reducer` 의 identifier로 적용되기 때문에 오타 혹은 존재 여부를 잘 파악해 작성해야한다는 점이다.

</aside>

```jsx
export const INCREMENT = 'increment';

const initialState = { counter: 0, showCounter: true };
// 이 처럼 초기 값을 따로 빼내어 사용할 수도 있다.
const counterReducer = (state = { initialState }, action) => {
    if(action.type === INCREMENT) {
        return {
            counter: state.counter + 1,
            showCounter: state.showCounter,
        };
		}
    return state;
};
```

```jsx
import { INCREMENT } from '../store/store';

const Counter = () => {
  const dispatch = useDispatch();

  const counter = useSelector(state => state.counter);
  const show = useSelector(state => state.showCounter);

  const incrementHandler = () => {
    dispatch({ type: INCREMENT });
  };
}
```

<aside>
♦️ 따라서 위와 같이 action의 type을 동기화하며 사용하는 방법도 있다.

</aside>

---
<br><br>
## 마무리

useState로 상태관리를 하면서 끌어올리기, 상태의 관리에 어려움을 느끼던 찰나에 리덕스를 학습하게 되었는데 처음 접하는 입장에서 개념을 바로잡긴 힘든것 같다.

꾸준히 학습해서 내걸로 만드는 노력이 필요하다.
