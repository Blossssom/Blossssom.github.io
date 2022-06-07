---

layout: post

title: Component

subtitle: SEB TIL

categories: coding

tags: [javascript, React]

---
## 컴포넌트 란?

---

<aside>
리액트에서 컴포넌란 가장 중요한 요소이다. 리액트로 만들어진 앱의 최소한의 단위가 컴포넌트이기 때문이다. 이 작은 컴포넌트들이 유기적으로 잘 연결되고 동작될때 같은 리액트 앱이라도 더욱 효율적인 앱이 될 수 있다. 즉, 단단하고 작은 컴포넌트들이 유기적으로 연결되어 동작할 때 좋은 앱이 만들어진다.

컴포넌트는 데이터를 입력받아 DOM Node를 반환하는 함수이다. 이때 입력받는 데이터는 props나 state 같은 것들이 있다.

</aside>

## Uncontrolled Component Vs Controlled Component

---

<aside>
Uncontrolled Component는 상태를 react에서 제어하지 않는다. 다른말로 하자면, 상태가 바뀌면서 re-rendering 되는 리액트의 특성을 뛰어넘어 rendering을 하지 않으며 이 부분에서 cost-efficient 하다고 할 수 있다. 그렇다면 어떠한 경우에 Uncontrolled Component를 사용할 수 있을까? 예를 든다면 <form>에 사용할 수 있을 것이다. 하지만 초기화 기능을 사용할 경우 Controlled Component를 써서 state를 refrash 해줘야 할 것이다.

</aside>

## Pure Component?

---

<aside>
pure component는 props나 state를 얕은 비교를 통해 변경점이 없으면 render를 다시 실행하지 않는다. 즉, props나 state의 변화가 없으면 rendering하지 않기 때문에, 훨씬 효율적이다. 예를 들어 pure component로 form을 구성하는 이메일과 비밀번호의 컴포넌트를 만들었다면, 이메일이 바뀌었을때 email pure component가 실행되지만 password pure component는 실행되지 않아 효율적이다. 

하지만 pure component만 사용한다면 불필요한 비교로직이 들어가 비효율적일 수 있다.

</aside>

```jsx
<Input
type="password"
...
onChange={{{target: {value}}) => this.setState({password: value })}
/>
```

<aside>
위 component가 실행되며 렌더링 되면서 함수 인스턴스가 새로 생성된다. 즉, `onChange` 가 무조건 콜된다는 것이다. 무조건 인풋 컴포넌트가 리렌더링되는 것은 비효율적이다.

</aside>

<aside>
컴포넌트는 각 부분부분을 쪼개 구현하는 가장 작은 단위이다.

</aside>

![Untitled](/post-img/component01.png)

<aside>
컴포넌트, css를 구분하는 방법은 다양하지만 우선 src > component 폴더를 생성해 `Hello.jsx` 파일을 생성하였다.

</aside>

```jsx
function Hello() {
    <p>Hello</p>
}

export default Hello;
```

<aside>
단촐하게 문단하나를 생성해 내보내는 `Hello.jsx`를 작성하였다.

</aside>

```jsx
import './test.css';
import Hello from './component/Hello';

function App() {

  return (
    <div className="App">
      <Hello />
    </div>
  );
}

export default App;
```

<aside>
위 처럼 다른 컴포넌트에서 불러올 때는 `import` 를 사용해 불러오며 태그명을 불러온 컴포넌트 명으로 지정하여 사용한다. 

중간에 다른 내용이 없을 때는 열린 태그로 마무리 해주는 것이 깔끔하다. 하지만 이 코드는 에러를 발생시키는데 함수형 컴포넌트는 반드시 `return` 을 사용해 `jsx` 를 반환해야하기 때문이다.

</aside>

```jsx
function Hello() {
    return(
        <p>Hello</p>
    );
}

export default Hello;
```

<aside>
`Hello.jsx` 를 이렇게 반환해주면 정상적으로 동작한다.

</aside>

```jsx
function Welcome() {
    return(
        <h2>Welcome!</h2>
    );
}

export default Welcome;
```

<aside>
이번엔 `Welcome.jsx` 파일을 생성해 보자

</aside>

```jsx
import './test.css';
import Hello from './component/Hello';
import Welcome from './component/Welcome';

function App() {

  return (
    <div className="App">
      <Welcome />
      <Hello />
    </div>
  );
}

export default App;
```

<aside>
이렇게 각 컴포넌트가 하나의 컴포넌트에 모여 작성되는 것을 간단하게 구현할 수 있다.

이번엔 더 작은 단위의 컴포넌트를 작성해서 `Hello` 컴포넌트에 넣어보자

</aside>

```jsx
function World() {
    return(
        <p>World!!</p>
    );
}

export default World;
```

```jsx
import World from "./World";

function Hello() {
    return(
        <div>
            <p>Hello</p>
            <World />
        </div>
    );
}

export default Hello;
```

<aside>
아까와는 조금 다른 부분이 있다. 빈 `div` 태그를 만들어 감싸줬는데 **컴포넌트는 반드시 하나의 태그 묶음을 반환해야한다.** 따라서 빈태그 `<>` 라도 사용해 묶어서 반환해야 에러를 발생시키지 않는다.

</aside>

```jsx
import './test.css';
import Hello from './component/Hello';
import Welcome from './component/Welcome';

function App() {

  return (
    <div className="App">
      <Welcome />
      <Hello />
      <Hello />
    </div>
  );
}

export default App;
```

<aside>
또한 이렇게 만든 컴포넌트는 중복적으로 어디서든 호출하여 사용이 가능하다. 이전과 달리 같은 부분을 매번 작성할 필요가 없어졌다.

</aside>

---

<br><br><br>

