---

layout: post

title: 클로저 (Closure)

subtitle: JS Study

categories: coding

tags: [javascript]

---

## 클로저 란?

클로저(closure)는 함수와 그 함수가 선언됐을 때의 렉시컬 환경(Lexical environment)과의 조합이다.

말이 좀 어려우니 간단하게 말하자면 내부함수가 외부함수의 변수에 접근할 수 있는 기능을 말한다.

[자바스크립트 클로저](https://brunch.co.kr/@kd4/10)

```jsx
const addFunc = (x) => {
  return (y) => {
    return x + y;
  };
};

const result = addFunc(5);
console.log(result(4));
```

위 코드를 보면 함수가 `addFunc()` 함수가 실행되며 렉시컬 환경이 생성되고 x의 값으로 5가 할당되었다. 

후에 `result` 변수에 `addFunc()` 를 호출해 내부함수를 할당해 주었고 다시한번 호출하였다. 이렇게 내부함수를 호출하게 되면 내부 익명함수의 렉시컬 환경에 4라는 값이 들어가 최종 반환 값인 `x + y` 를 반환하게 된다. 

이렇게 내부 > 외부 > 전역 식이로 각 렉시컬 스코프를 참조하는 현상을 클로저라고 한다. 물론 조금더 깊게 파고들면 단순히 이러한 말로 표현하는 것은 답이 아닐 수 있지만 간단하게 개념을 쌓아가며 깊게 들어가보자.

## 클로저의 활용

클로저는 여러 분야에서 사용할 수 있지만 프론트엔드 실무에서 자주 활용되는 모습을 보인다.

물론 일반 함수와 다르게 선언시 환경을 기억해야 하므로 메모리적인 손해는 날 수 있지만 그 이상의 활용을 볼 수 있다.

### 1. 상태유지

선언 시의 환경을 기억한다는 것은 상태를 유지하고 최신 상태로 반영할 수 있다는 강점을 가진다.

```jsx
<!DOCTYPE html>
<html>
<body>
  <button class="toggle">toggle button</button>
  <div class="txt">
    <h1>toggle test</h1>
  </div>

  <script>
    let txtField = document.querySelector('.txt');
    let toggleBtn = document.querySelector('.toggle');

    let toggle = (function () {
      let isVisable = false;

      // 1.클로저를 반환
      return function () {
        txtField .style.display = isVisable ? 'block' : 'none';
        // 3. 상태 변경
        isVisable = !isVisable;
      };
    })();

    // 2. 이벤트 프로퍼티에 클로저를 할당
    toggleBtn.onclick = toggle;
  </script>
</body>
</html>
```

간단한 toggle 버튼 구현 코드를 살펴보자.

`toggle()` 로 선언된 함수는 즉시실행 함수로 곧바로 실행되며 내부함수를 반환하고 즉시 소멸될 것이다. 이때 내부함수는 `toggle()` 내에 선언된 `inVisable` 을 참조하고 있어 클로저가 된다.

이러한 클로저를 click 이벤트에 할당하게 되면 버튼 클릭 마다 클로저가 호출될 것이고, 즉시 실행함수 내부의 `inVisable` 또한 최신 상태를 유지할 수 있어 값을 변경할 수 있게된다.

### 2. 데이터 은닉화

전역변수는 전역 혹은 외부 스크립트에서도 간단하게 접근이 가능해 의도치 않은 값의 변경, 중복 등 여러 치명적인 오류를 야기할 수 있어 최대한 사용을 지양해야한다. 

단순히 사용을 하지 않는 방법도 있지만 생각처럼 그렇게 간단하지 않은 경우도 많기에 클로저를 활용해 전역변수의 문제를 해결할 수 있는 방법을 알아보자.

```jsx
<!DOCTYPE html>
<html>
  <body>
  <button id="plus">+</button>
  <p id="count">0</p>
  <script>
    let plusBtn = document.getElementById('plus');
    let countTxt = document.getElementById('count');

    let plus= (function () {
      // 카운트 상태를 유지하기 위한 자유 변수
      let count = 0;
      // 클로저를 반환
      return function () {
        return count++;
      };
    }());

    plusBtn.onclick = function () {
      countTxt.innerHTML = plus();
    };
  </script>
</body>
```

위에서 살펴본 toggle 함수처럼 상태를 유지하며 반환하는 클로저 함수이다. 

즉시 실행되며 선언시 참조한 `count` 값을 기억하고 있어 기존 `count` 값에 1을 더한 값을 반환하는데 일반함수와 달리 호출시 마다 `count` 가 0으로 재할당 되지 않고 바로 직전 값을 유지하고 있다는 것이다. 

이렇게 되면 전역변수를 사용하는 코드보다 안전하게 기능을 구현할 수 있다. 특히 실무에서는 안정성을 더욱 따져야하기에 반드시 알아두어야할 요소일 것이다.

이렇게 상태를 유지하는 경우는 즉시 실행함수와 함께 사용해야하나 다른 방법이 있는지도 살펴봐야겠다.

---


## 마무리

클로저를 조금 당연하게 생각하고 있었는데, 다른 예시를 보니 내가 생각했던 것보다 더 많은 활용성이 있는 것 같다.

특히 상태유지 부분은 아직 활용해보지 못했기에 어떠한 방식으로 동작하는지 더 알아보고 활용해 보아야겠다.

<br><br><br>

<!-- ## 참고 사이트
- [태기의 개발 Blog](https://ljtaek2.tistory.com/140) -->
