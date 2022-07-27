---

layout: post

title: 22 / 07 / 22 My TIL [useRef & useImperativeHandle]

subtitle: SEB TIL

categories: coding

tags: [Hooks]

---
# useRef 활용과 useImperativeHandle

<aside>
♦️ 로그인 form에서 submit 동작 시 조건에 만족하지 않는 input 목록에 focus를 주기 위해 useRef를 사용해 처리해보자.

</aside>

### Login.jsx

```jsx
const submitHandler = (event) => {
    event.preventDefault();
    if(formIsValid) {
      authCtx.onLogin(emailState.value, passwordState.value);
    }else if(!emailValid){
      emailInputRef.current.activate();
    }else if(!passwordValid) {
      passwordInputRef.current.activate();
    }else {
      nicknameInputRef.current.activate();
    }
  };

  return (
    <Card className={classes.login}>
      <form onSubmit={submitHandler}>
        <Input ref={emailInputRef} ids='email' label='E-Mail' type='email' isValid={emailValid} changeHandler={emailChangeHandler} validateHandler={validateEmailHandler} value={emailState.value} />
        <Input ref={passwordInputRef} ids='password' label='Password' type='password' isValid={passwordValid} changeHandler={passwordChangeHandler} validateHandler={validatePasswordHandler} value={passwordState.value} />
        <Input ref={nicknameInputRef} ids='nickname' label='NickName' type='text' isValid={nicknameValid} changeHandler={nicknameChangeHandler} validateHandler={validateNicknameHandler} value={nicknameState.value} />
        
        <div className={classes.actions}>
          <Button type="submit" className={classes.btn}>
            Login
          </Button>
        </div>
      </form>
    </Card>
  );
```

### Input.jsx

```jsx
import React, {useRef} from 'react'
import classes from './Input.module.css';

export default function Input({isValid, type, changeHandler, validateHandler, value, label, ids}) {

    const inputRef = useRef();

    const activate = () => {
        inputRef.current.focus();
    };

  return (
    <div className={`${classes.control} ${isValid === false ? classes.invalid : ''}`}>
        <label htmlFor={ids}>{label}</label>
        <input
            ref={inputRef}
            id={ids}
            type={type}
            onChange={changeHandler}
            onBlur={validateHandler}
            value={value}
        />
    </div>
  )
}
```

우선 form의 submit 동작시 useRef를 사용하기 위해 Input 컴포넌트에 ref를 할당하였다. 
submitHandler의 조건문을 보면 `activate` 메서드를 사용한 것을 볼 수 있는데, 이는 Input 컴포넌트에 선언한 함수로 마치 내장 객체를 사용하는 것 처럼 보일 수 있다.

다만 이런식으로 useRef를 사용할 경우 문제가 생긴다.

1. Input 컴포넌트에서 props로 받은 ref를 사용하지 않고 있다.
2. Input에서 props를 사용하여도 ref는 key와 같이 일종의 예약어로서 일반적인 props와 같이 접근할 수 없다.

따라서 위와 같이 컴포넌트에 ref를 전달할 때는 추가적인 작업이 필요하다.

## useImperativeHandle

<aside>
♦️ useImperativeHandle은 react에서 제공하는 hook으로 컴포넌트 혹은 컴포넌트 외부에서 오는 기능들을 명령적으로 사용할 수 있게 해준다.

즉, 부모 컴포넌트의 state를 통해 제어하지 않고 프로그래밍 적으로 컴포넌트에 직접 호출하거나 조작해서 사용하게 한다. 일반적인 react의 제어와 다르게 흘러가는 만큼 보기 드물기도 하며 자주 사용해서도 안되는 hook이다.

</aside>

```jsx
import React, {useRef, useImperativeHandle} from 'react'
import classes from './Input.module.css';

const Input = React.forwardRef(({isValid, type, changeHandler, validateHandler, value, label, ids}, ref) => {

    const inputRef = useRef();

    const activate = () => {
        inputRef.current.focus();
    };

    useImperativeHandle(ref, () => {
        return {
           focus: activate 
        } ;
    });

  return (
    <div className={`${classes.control} ${isValid === false ? classes.invalid : ''}`}>
        <label htmlFor={ids}>{label}</label>
        <input
            ref={inputRef}
            id={ids}
            type={type}
            onChange={changeHandler}
            onBlur={validateHandler}
            value={value}
        />
    </div>
  )
})

export default Input;
```

위 코드를 보면 일반 함수형 컴포넌트에서는 지정할 수 없는 내부 메서드 처럼 `activate` 를 사용하는 것이 보이는데 이러한 이유 때문에 `useImperactiveHandle` hook을 사용하는 것이다.

생각해보면 단순히 input 태그의 focus만을 위한다면 forwardRef만으로 감싸 처리할 수 있었다.

굳이 코드를 추가해가며 hook을 쓰는 이유가 당연히 있을 것이다. 

### 부모 컴포넌트에서 ref를 넘기지 않았을 경우

<aside>
♦️ 부모 컴포넌트에서 깜빡하고 ref를 넘기지 않는다면 해당 컴포넌트의 ref는 null을 반환하여 에러를 발생할 것이다. 따라서 상황에 맞춰 다음과 같은 코드를 작성할 수도 있다.

</aside>

 

```jsx
const FormControl = React.forwardRef((props, ref) => {
 // store Ref
const tempRef = useRef();
const [inputRef, setInputRef] = useState(ref || tempRef);
 return (
  <div className="form-control">
    <input type="text" ref={inputRef} />
    <button
     onClick={() => {
      inputRef.current.focus();
     }}>
     focus
    </button>
  </div>
 );
})
```

state를 활용해 ref의 여부를 따지고 동작을 수행하도록 만들 수 있다.

이 문제를 `useImperactiveHandle` 을 사용하여 작성해보자.

```jsx
const FormControl = React.forwardRef((props, ref) => {
 const inputRef = useRef(null);
 // useImperativeHandle 은 reference를 맵핑한다.
 useImperativeHandle(ref, () => inputRef.current);
 return (
  <div className="form-control">
    <input type="text" ref={inputRef} />
    <button
     onClick={() => {
      inputRef.current.focus();
     }}>
     focus
    </button>
  </div>
 );
})
```

위 코드를 살펴보면 `useImperactiveHandle` 의 arg의 첫번 째는 부모가 보낸 ref, 두 번째는 자식이 그에 맞춰보낼 ref로 이는 부모 컴포넌트가 ref.current에 접속할 수 있도록 맵핑하는 과정이다.

즉, 자식의 실제 ref를 보내지 않고 커스텀한 ref를 보낼 수 있다는 의미이다. 실제로 상단 예시코드를 보면 ref를 객체로 구성해 함수를 내보내고 있는 것을 볼 수 있다.

이는 함수에서 가질 수 없는 내부 method를 외부에 전달할 수 있다는 의미와 함께 컴포넌트를 private 그대로 남겨둘 수 있다는 의미이다.

```jsx
const FormControl = React.forwardRef((props, ref) => {
 const inputRef = useRef(null);
 useImperativeHandle(ref, () => ({getValue(){
  return inputRef.current.value
 }) }, [inputRef]); // deps도 추가가능하다.
 return (
  <div className="form-control">
    <input type="text" ref={inputRef} />
  </div>
 );
})
const App = () => {
 const inputRef = React.useRef();
 return (
  <div className="App">
    <FormControl ref={inputRef} />
    <button
     onClick={() => {
      console.log(inputRef.current.getValue());
     }}>
     focus
    </button>  
  </div>
 );
}
```

따라서 부모는 자식 컴포넌트의 DOM에 직접적으로 접근하는 것이 아니라 `useImperactiveHandle` 로 전달된 메서드에만 접근이 가능해져 컴포넌트 간의 독립성을 보장할 수 있는 장점이 있다.

하지만 이러한 상황이 일반적으로 자주 사용되지 않을 뿐더러 권장하지 않는 방식으로 사용을 지양하되 특정상황에 유용하게 사용될 수 있으니 알아두자.

---
<br><br>
## 마무리

scroll, focus 등을 조작할 때 useRef와 fowardRef를 사용했지만 다른 활용법이나 의문을 가진적이 없던것 같다.

요즘 만들어보는 재미에 이런저런 토이 프로젝트를 진행했지만 지금 생각해보니 학습이 아닌 반복이었다는 느낌이 든다. 생각하고 설계하고 계속해서 의심해야하는데 자꾸 샛길로 빠지는 느낌이 든다.

계속 생각하고 문제를 받아들이자.
