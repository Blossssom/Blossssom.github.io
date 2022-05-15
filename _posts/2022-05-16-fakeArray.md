---

layout: post

title: 배열과 유사배열

subtitle: JS Study

categories: coding

tags: [javascript]

---
# 배열과 유사배열

기존에 사용하던 배열과 모양은 비슷하지만 실질적으로 다른 유사배열을 알아보자.

```jsx
const arr = [1, 2, 3, 4, 5];
const nodes = document.querySelectorAll("div");
const els = document.body.children;
```

위 코드의 `nodes` 와 `els` 는 프론트엔드 개발을 하며 자주볼 수 있는 흔한 셀렉터 코드이다.
실제 출력을 해보면 `[]` 에 둘러쌓여있어 겉만봐서는 배열과 차이를 못느낄 것이다.

```jsx
Array.isArray(arr);  // true
Array.isArray(nodes);  // false
Array.isArray(els);  // false
```

하지만 배열을 판단하기위해 `isArray()` 를 실행해보면 직접 리터럴로 선언한 배열만 실제 배열로 판단되는데 이처럼 겉모습은 배열이지만 배열 아닌 객체를 **유사배열** 이라 한다.

이러한 유사배열이 어떻게 만들어지는지 알아보자.

```jsx
let fakeArr = {
  0: "a",
  1: "b",
  2: "c",
  length: 3
};
```

위에 선언한 `fakeArr` 객체가 바로 유사배열이다. 배열도 객체이고, `length` 속성도 가지고 있어 배열같이 인덱스와 length 속성을 사용할 수 있다.

하지만 유사배열을 구분해야하는 이유가 여기서 나오는데, 직접 선언한 length 외에 배열의 메서드를 사용할 수 없기 때문이다.

```jsx
arr.forEach((el) => {
  console.log(el);  // 1, 2, 3
});

els.forEach((el) => {
  console.log(el);  // Uncaught TypeError: els.forEach is not a function
});
```

`arr` 은 배열이기 때문에 `forEach()` 사용이 가능하지만 `els` 의 경우 유사배열이기에 에러가 발생한다. 상단에서 선언한 `querySelector` 로 받아온 `nodes` 의 경우에는 prototype에 forEach가 있어 사용할 수 있다. (단, getElementByClassName과 같은 셀렉터의 경우 forEach가 없다.)

이렇게 유사배열에서 메서드를 빌려쓰는 방법이 있다. 배열 prototype에서 `call` 이나 `apply` 를사용해 `forEach` 를 빌려오는 것이다.

```jsx
Array.prototype.forEach.call(els, (el) => {
  console.log(el);
});

[].forEach.call(els, (el) => {
  console.log(el);
});
```

이렇게 유사 배열의 경우에도 `forEach` 를 사용할 수 있다. 또한 `filter, reduce` 등 다른 배열의 메서드도 사용 가능하다.

최신 자바스크립트 문법에서는 보다 쉽게 사용할 수 있다.

```jsx
Array.from(nodes).forEach((el) => { console.log(el) });
```

또한 `function` 의 `arguments` 의 경우에도 유사배열이므로 프로토타입에서 메서드를 빌려와 사용해야한다.


---


## 마무리

DOM 활용을 공부하면서 이것저것 실험하다 배열인데 메서드를 사용할 수 없는 상황을 만났다.

이유를 찾다찾다 이 글까지 포스팅하게 되었는데 유사배열을 자주 만나는 프론트엔드에서 반드시 알아둬야하는 파트였던것 같아 다행이다.

다만 일부 메서드의 동작이 생각과 달리 흘러가는 부분이 있어 추가적으로 찾아봐야할 것 같다.

<br><br><br>

## 참고 사이트
- [제로초 Blog](https://www.zerocho.com/category/JavaScript/post/5af6f9e707d77a001bb579d2)
