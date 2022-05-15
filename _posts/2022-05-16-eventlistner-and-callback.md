---

layout: post

title: 이벤트 리스너와 콜백

subtitle: JS Study

categories: coding

tags: [javascript]

---
# 이벤트 리스너와 콜백

프론트엔드를 학습하며 눈돌리면 보이는 이벤트 리스너와 콜백 방식을 알아보자.

## 이벤트 리스너 (event listener)

---

이벤트 리스너는 말 그대로 해당 이벤트에 대해 대기하는 것을 뜻한다. 해당 이벤트가 발생하면 등록했던 이벤트 리스너가 실행된다.

```jsx
window.onload = function () {
  alert("Hello bloxxom");
};
```

여러 블로그의 예시 코드들을 보았다면 꽤나 익숙한 코드일 것이다.
이 코드가 대표적인 이벤트 리스너인데, `window` 가 `load` 될 때 function 부분이 실행되는 것이다. 브라우저가 `load` 되었다는 것을 알려주고 이후 함수가 실행된다.

비슷하게 자주 볼 수 있는 코드로는 `onclick` 이 있다. 

이벤트 리스너의 경우 항상 `on + 이벤트 명` 으로 명명되니 알아두자.

자주 쓰이는 이벤트 목록을 잠깐 살펴보면 다음과 같다.

- onblur (객체가 focus를 잃었을 때)
- onchange (객체의 내용이 바뀌고 focus를 잃었을 때)
- onclick (객체를 클릭했을 때)
- ondblclick (더블클릭할 때)
- onerror (에러가 발생했을 때)
- onfocus (객체에 focus가 되었을 때)
- onkeydown (키를 눌렀을 때)
- onkeypress (키를 누르고 있을 때)
- onkeyup (키를 눌렀다 뗐을 때)
- onload (문서나 객체가 로딩되었을 때)
- onmouseover (마우스가 객체 위에 올라왔을 때)
- onmouseout (마우스가 객체 바깥으로 나갔을 때)
- onreset (reset 버튼을 눌렀을 때)
- onresize (객체의 크기가 바뀌었을 때)
- onscroll (스크롤 바를 조작할 때)
- onsubmit (폼이 전송될 때)

이외에도 여러 이벤트가 있으니 사용목적에 따라 찾아보자.

```jsx
document.getElementById("container").onclick = () => {
  alert("click!");
};
```

window 뿐만 아니라 위 처럼 여러 태그, 클래스와 같은 객체에 각각 이벤트를 설정할 수 있다. 모든 DOM들이 이벤트 리스너를 등록할 수 있는 속성을 가지고 있으며, pc에서 동작할 수 있는 간단한 이벤트는 거의 모두 존재한다.

이벤트를 붙이는 다른 방법으로 `addEventListner` 가 있다.
방식 자체는 조금 더 복잡한 느낌이지만 여러 이벤트를 등록할 수 있고, `removeEventListner` 를 사용해 특정 이벤트를 제거할 수 있기에 해당 방식을 추천한다.

```jsx
function onClick() {
  alert('I\'m clicked!');
}
function onClick2() {
  alert('또다른 이벤트');
}
document.getElementById('clickMe').addEventListener('click', onClick); // 이벤트 연결
document.getElementById('clickMe').addEventListener('click', onClick2); // 또 하나의 이벤트 연결
document.getElementById('clickMe').removeEventListener('click', onClick); // 연결할 이벤트 중 하나 제거
```

## 콜백 (callback)

---

위 코드에서 function 부분을 **콜백(callback)** 이라 한다. 말그대로 다시 전화를 준다는 것인데, 이벤트가 실행됐을 때, 사용자에게 다시 알려준다는 의미이다.

혹은 나중에 호출한다는 의미로 사용되기도 하는데, 직접 코드를 보면서 알아보자.

```jsx
let func = function(callback) {
	console.log('hello');
	callback();
}

let callback = function() {
	console.log('this is call back');
}

func(callback);

// hello
// this is call back
```

위 코드처럼 func 함수가  먼저 호출되고 나중에 callback 함수가 호출된다.

자주 쓰이는 코드로 좀 더 알아보자.

```jsx
// button 태그요소들을 Select한다
const button = document.querySelector('button')

// clicked 클래스를 요소에 추가하는 함수이다.
function clicked (e) {
  this.classList.add('clicked')
}

// click 함수를 콜백 함수로써 event listener에 등록한다.
button.addEventListener('click', clicked)
```

위 처럼 버튼에서 click 이벤트를 수신하도록 만들었을 때, 버튼 클릭이 감지될 경우 javascript는 `clicked` 함수를 실행해야한다.

이 경우 **clicked는 callback 함수이고, addEventListner는 callback을 accept하는 함수이다.**  

다음 예제는 어떻게 callback 함수와 callback accepting 함수를 작성해야하는지에 대해 나타낸 것이다.

```jsx
// callback 함수를 수용할 수 있는 callback Accepting 함수를 만들었다.
const callbackAcceptingFunction1 = function(fn){
    // 1,2,3 인수를 사용하여 fn함수를 호출한다.
    return fn(1, 2, 3)
}
const callbackAcceptingFunction2 = function(fn){
    // 1,2 인수를 사용하여 fn함수를 호출한다.
    return fn(1, 2)
}
// callback 함수를 정의했다.
const callback = function(arg1, arg2, arg3){
    return arg1 + arg2 + arg3
}

// callback accepting 함수에게 callback을 전달했다.
const result1 = callbackAcceptingFunction1(callback)
// 1, 2, 3의 인자를 가지고 callback 함수를 call 했다.
console.log(result1) //6

const result2 = callbackAcceptingFunction2(callback)
// 1, 2, undefined의 인자를 가지고 callback 함수를 call 했다.
console.log(result2) // NAN : 1 + 2 + undefined
```

이것이 callback의 구조(anatomy)이다.

```jsx
button.addEventListener('click', function(event){
	event.prventDefault();
});
```

`addEventListner` 에 event argument를 사용해 기초적인 callback 함수의 기초적인 idea를 볼 수 있다. 

조금 단순하게 **콜백은** **다른 함수(Callback Accepting function)에 함수(Callback)를 전달한다.** 라는 사실만 기억하며 학습해보자.

또한 callback 함수를 callback Accepting에 인자로써 전달할 수 있는 이유는 **javascript에서 함수는 1급 객체이자 1급 함수**이기 때문이다.  ****

### 콜백을 왜 사용하는가

콜백 함수는 2가지 방식으로 사용된다.

1. 동기적 함수
2. 비동기적 함수

### 1. 동기적 함수로써의 Callback

**동기적(Synchronous)**

코드가 위에서 아래로, 오른쪽으로 순차적으로 실행되고 다음줄의 코드가 실행되기 전에 현재 줄의 코드가 완료될 때 까지 기다리는 방식을 의미한다.

```jsx
const addOne = function(n){
	console.log(n+1);
}

addOne(1);	// 2
addOne(2);	// 3
addOne(3);	// 4
addOne(4);	// 5
```

간단한 예제를 보면 `addOne` 함수는 차례대로 마지막 줄이 실행될 때 까지 진행된다.

**동기적 콜백함수는 코드의 일부를 다른코드로 쉽게 교체하려는 경우에 사용된다.**

동기적 콜백함수의 예시를 더 살펴보자.

배열의 `Array.filter` 함수를 보면 전해지는 callback 함수를 재사용하여 다양한 원소를 가지게는 새로운 배열을 생성할 수 있다.

```jsx
const numbers = [3, 4, 10, 20];

const getLessThanFive = function(num){
	return num<5;
}

const getMoreThanTen = function(num){
	return num>10;
}

// numbers의 filter함수에 getLessThanFive 함수를 전달해준다.
const LessThanFive = nubmers.filter(getLessThanFive);
console.log(lessThanFive);	// [ 3, 4 ]

// numbers의 filter함수에 getMoreThanTen 함수를 전달해준다.
const MoreThanTen = numbers.filter(getMoreThanTen)
console.log(MoreThanTen);	// [ 20 ]
```

위 예제는 우리가 콜백을 동기적으로 사용하는가에 대한 대표적인 예제이다.

### 2. 비동기적 함수로써의 Callback

**비동기적(Asynchronous)**

비동기 방식은  javascript가 완료될 때까지 기다려야하는 경우 대기하는 동안 제공된 나머지 작업을 실행하는 방식을 의미한다.

비동기적 함수의 대표적인 예시는 `setTimeout` 함수이다. setTimeout 함수는 callback 함수를 주어진 시간 뒤에 실행한다.

```jsx
const tenSecondLater = function(){
    console.log("ten seconds passed!");
}

setTimeout(tenSecondLater,10000);
console.log('start!')
```

```jsx
Start! (즉시)
10 seconds passed! (10초 후)
```

위 코드에서 setTimeout 함수는 10초간 대기 후 `"ten seconds passed!"` 라는 로그를 발생시킨다.

그리고 callback을 기다리는 동안 javascript는 `"start!"` 로그를 실행한다.

**비동기 작업은 동기적 작업에 비해 매우 복잡한데 왜 비동기 callback 함수를 사용할까?**

비동기 작업이 왜 중요한지 생각하려면 일상적에서 일을 처리하는 방식을 생각해보자.
만약 우리가 한 번에 하나의 작업만을 처리할 수 있다면 (이것을 **single-Thread** 라고한다.)
음식을 주문한 후 음식이 도착할 때 까지 문앞에서 기다리며 다른 작업은 처리할 수 없을 것이다.

시작한 작업을 처리한 후에야 비로소 다른 작업을 진행할 수 있는 것이다. (이러한 행동을 **blocking** 이라 하며, 무언가를 기다리는 동안 다른 작업들은 **blocked** 되어진다.)

```jsx
const orderPizza = function(flavour){
	callPizzaShop(`I want a ${flavour} pizza`);
	wait20minsForpizzaToCome(); // 이 함수에선 어떠한 것도 일어날 수 없다.
	bringPizzaToYou();
}

orderPizza('hawaiian');

// 피자 주문을 완료하고 난 뒤 시작하는 2가지의 작업들
mopFloor();
ironClothes();
```

위 코드에서의 blocking 작업은 매우 비효율적이다.

이제 이러한 예시를 브라우저의 문맥으로 살펴보자.
브라우저의 버튼이 클릭될 때 버튼의 색깔을 바꾸도록 작업을 진행해보는 것이다.

만약 동기적 방식으로 진행한다면 우리는 버튼을 클릭할 때 까지 오는 모든 명령을 무시하고 오직 버튼의 이벤트만을 감시할 것이다. 이러한 문제를 해결하기 위해 비동기적 방식을 사용하는 것이다.

**이것이 Javascript에서 비동기 프로그래밍이 핵심인 이유이다.**

비동기 작업 동안에 무엇이 일어나는지에 대해서 알기위해 **Event Loop**에 대해 알아 볼 필요가 있다.

---

**이벤트 루프 (Event Loop)**

이벤트 루프를 계획하기 위해 Javascript를 **할 일 목록**(todo-list)을 다루는 집사라고 생각해보자.
이 목록에는 사용자가 지시한 모든 것이 포함되어있으며, Javascript는 사용자가 지정한 순서대로 목록을 하나씩 살펴본다.

```jsx
const addOne = function(n){
	return n+1;
}

addOne(1) // 2
addOne(2) // 3
addOne(3) // 4
addOne(4) // 5
addOne(5) // 6
```

아래 목록은 Javascript의 todo-list이다.

<img src='https://i.imgur.com/sbkvtIP.png' width='300px'/>

Javascript는 todo-list 외에도 대기해야 할 항목을 추적하는 **대기목록(waiting-list)** 을 유지한다. 만약사용자가 Javascript에게 음식을 주문하라고 시킨다면, 자바스크립트는 가게에 음식을 주문하고 **음식이 올 때 까지 기다리라는 명령을 대기목록(waiting-list)에 추가**할 것이다.

또한 **그러는 동안에 todo-list에 이미 있는 명령들은 실행될 것이다.**

```jsx
const orderPizza (flavor, callback) {
  callPizzaShop(`I want a ${flavor} pizza`)

  // Note: 아래의 코드 3줄은 가상의 실제 javascript 코드가 아닌 가상의 코드이다.
  whenPizzaComesBack {
    callback()
  }
}

const layTheTable = function(){
  console.log('laying the table')
}
orderPizza('Hawaiian', layTheTable)
mopFloor()
ironClothes()
```

위 예시에서 Javascript의 초기 todo-list는 다음과 같다.

<img src='https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FpVZoN%2FbtqzFVdLur5%2FqIqIwUWX1wxXoBMeV68rq1%2Fimg.png' width='300px'/>

Javascript는 orderPizza명령을 시행하는 동안에 pizza가 도착하기를 기다려야 한다는 것을 안다.

그래서 Javascript는 **"Waiting for pizza to arrive" 명령**을 나머지 작업을 처리하는 동안에 **대기 목록(waiting-list)에 추가**한다.

<img src='https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2Fb0HR7h%2FbtqzFWDKIv5%2FB94QjL7tRokjyWfFGWjkT1%2Fimg.png' width='600px'/>

피자가 도착했을 때, Javascript는 초인종으로부터 알림을 받고 **다른 일들을 마치면 layTheTable**
함수를 실행 할 **mental note** 를 만든다.

<img src='https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2Fb6ahh5%2FbtqzFAnrqp8%2FX4k1K9JazYJp6JQuOrqxo1%2Fimg.png' width='500px'/>

javascript는 mental note에 명령을 추가함으로써 layTheTable 명령을 실행할 필요가 있다는 것을 알게 된다.

일단 Javascript가 다른 작업들을 마치면, 콜백 함수인 layTheTable을 실행한다.

<img src='https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FdUa0I4%2FbtqzGrpZySB%2FjOMbyo8x1aCbyRxQCd8fU1%2Fimg.png' width='500px'/>

다른 모든 작업들이 끝나면 Javascript는 위와 같이 Table을 배치한다.

이러한 작업들을 **이벤트 루프**라고 하며, 이벤트 루프에 대한 모든것을 이해하기위해 실제 키워드를 집사에 비유해 대체할 수 있다.

- **할 일 목록(Todo-list)** -> Call stack
- **대기 목록(Waiting-list)** > Web apis
- **Mental note** > Evenet queue

<img src='https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FcDpZTB%2FbtqzFeZckuJ%2Fv90Ro6nQYTP9ETJpNv8kK1%2Fimg.png' width='500px'/>

---


## 마무리

작업의 순서만 생각하다보니 비동기적 방식에 대해 크게 신경쓰지 않고 코드를 짜고있었는데 자주 쓰는 코드의 실행방식이나 타이밍에 대해 생각하면서 코딩을 할 필요성을 느꼈다. 

생각없이 무지성으로 짜는 코드가 무슨의미가 있나 싶으면서도 평소에 그러고 있다는 느낌이 들어 뜨끔하는 파트였다.

<br><br><br>

## 참고 사이트
- [제로초 Blog](https://www.zerocho.com/category/JavaScript/post/5af6f9e707d77a001bb579d2)
