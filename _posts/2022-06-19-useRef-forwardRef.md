---

layout: post

title: useRef & forwardRef

subtitle: SEB TIL

categories: coding

tags: [javascript, React]

---

## useRef ?

`useRef` 는 리액트에서 특정 DOM을 가리킬 때 사용하는 Hook이다. 마치 `getElementById()` 처럼 DOM의 특정 요소를 선택할 수 있으며 focus, hover 등 특정 DOM 요소에 사용하는 이벤트 등을 설정하는데 활용되기도 하며, 비동기를 통해 만들어진 id와 외부 라이브러리를 사용하여 생성된 인스턴스, scroll의 위치 등에 사용되기도 한다.

useRef로 관리하는 변수는 값이 바뀌어도 컴포넌트를 리렌더링 하지 않는다.

### focus로 활용하기

```jsx
function TextInputWithFocusButton() {
  const inputEl = useRef(null);
  const onButtonClick = () => {
    // `current` points to the mounted text input element
    inputEl.current.focus();
  };
  return (
    <>
      <input ref={inputEl} type="text" />
      <button onClick={onButtonClick}>Focus the input</button>
    </>
  );
}
```

위 코드는 focus 이벤트를 구현한 코드이다. useRef를 설정한 후 접근할 DOM에 `ref` 요소를 사용해 지정하였다. 

이후 버튼의 클릭 이벤트를 만들어 `.current` 를 사용해 ref가 설정된 DOM에 focus를 주는 간단한 코드이다.

### 변수로 활용하기

```jsx
const test = () => {
		const [result, setResult] = useState('');
    const [imgCoord, setImgCoord] = useState(rspCoords.바위);
    const [score, setScore] = useState(0);
    const inverval = useRef();
    
    useEffect(()=> {
    	interval.current = setInterval(changeHand, 100);
        return () => {
        	clearInterval(interval.current);
        }
     }, [imgCoord]);
```

위 코드는 컴포넌트가 Mount 될 때 `useRef` 에 변수를 할당해 비동기 함수를 실행시키고 UnMount 시에 clearInterval로 clean up을 실행하는 코드이다. 이렇게 useRef를 활용해 변수를 관리하면 해당 변수가 업데이트 되어도 컴포넌트가 리렌더링 되지 않기 때문에 변수의 경우 useRef로 관리하는 것이 효율적이다.

## forwardRef

React에서 특수한 목적으로 사용되어 일반적인 용도로 사용할 수 없는 props가 몇가지 존재한다. `key` 가 가장 대표적인 예시이며, `ref` 또한 DOM에 접근하는 특수한 용도이므로 일반적인 props로 사용할 수 없다. 

따라서 해당 컴포넌트에서 HTML 엘리먼트가 아닌 다른 컴포넌트에 props로 전달하기 위해서는 `forwardRef` 를 사용해야한다.

컴포넌트를 `forwardRef` 로 감싸주면 해당 컴포넌트는 두 번째 매개변수를 갖게되어 이를 통해 외부에서 ref props를 넘길 수 있다.

```jsx
import React, { useRef } from "react";

function Input({ ref }) {
  return <input type="text" ref={ref} />;
}

function Field() {
  const inputRef = useRef(null);

  function handleFocus() {
    inputRef.current.focus();
  }

  return (
    <>
      <Input ref={inputRef} />
      <button onClick={handleFocus}>입력란 포커스</button>
    </>
  );
}
```

이처럼 일반적으로 ref를 props로 넘길경우 브라우저 콘솔에서 경고메세지를 띄우는데, ref는 일반 props가 아니기 때문에 `undefined` 가 할당되고 값을 전달할 수 없다. 

물론 ref1, refs 등 ref의 명칭만 바꿔 props로 전달할 수 있지만 컴포넌트 디자인에 혼란을 줄 수 있기 때문에 위와 같은 경우에는 forwardRef 사용하는 것을 지향해야한다.

```jsx
import React, { forwardRef, useRef } from "react";

const Input = forwardRef((props, ref) => {
  return <input type="text" ref={ref} />;
});

function Field() {
  const inputRef = useRef(null);

  function handleFocus() {
    inputRef.current.focus();
  }

  return (
    <>
      <Input ref={inputRef} />
      <button onClick={handleFocus}>입력란 포커스</button>
    </>
  );
}
```

## 다른 예시

```jsx
import React, { useEffect, useRef } from "react";
import "./App.css";
import Input from './components/Input.js';

function App() {
  // 선언 
  const firstNameRef = useRef(null);
  const lastNameRef = useRef(null);
  const submitRef = useRef(null);

  // 처음 렌더링 시 보여줄 효과 
  useEffect(() => {
    firstNameRef.current.focus();
  }, []);
  
  // 첫번째 칸 입력 후 엔터누르면 포커스가 다음 칸으로 넘어감 
  function firstKeyDown(e) {
    if (e.key === "Enter") {
      lastNameRef.current.focus();
    }
  }

  // 두번째 칸 입력 후 엔터누르면 포커스가 버튼으로 넘어감 
  function lastKeyDown(e) {
    if (e.key === "Enter") {
      submitRef.current.focus();
    }
  }

  // 버튼누르면 alert가 뜬다.
  function submitKeyDown() {
    alert("form submitted");
  }
  return (
    <div className="App">
      <header className="App-header">
        <Input
          type="text"
          onKeyDown={firstKeyDown}
          ref={firstNameRef}
          placeholder="enter first name"
        />
        <Input
          type="text"
          onKeyDown={lastKeyDown}
          ref={lastNameRef}
          placeholder="enter last name"
        />
        <button onKeyDown={submitKeyDown} ref={submitRef}>
          Submit
        </button>
      </header>
    </div>
  );
}

export default App;
출처: https://devbirdfeet.tistory.com/59 [새발개발자:티스토리]
```

```jsx
import React from "react";

function Input({ type, onKeyDown, placeholder }, ref) {
  return (
    <input
      ref={ref}
      type={type}
      onKeyDown={onKeyDown}
      placeholder={placeholder}
    />
  );
}
const forwaredInput = React.forwardRef(Input);
export default forwaredInput; 
출처: https://devbirdfeet.tistory.com/59 [새발개발자:티스토리]
```

---
<br><br><br>
## 마무리

useEffect, useState 등의 Hook 처럼 자주 사용해보지 못했지만 scroll, 변수 사용 등 여러 방면에서 활용할 수 있는 Hook인 것 같다. 더 많은 예시를 찾아보고 어떻게 활용할지 연습해볼 필요가 있다.


## 참고사이트
- [새발개발자](https://devbirdfeet.tistory.com/59)
- [proglish 블로그](https://proglish.tistory.com/179)

