---

layout: post

title: 고차함수 (higher order function)

subtitle: JS study

categories: coding

tags: [javascript]

---

# 고차함수 (higher order function)

고차함수는 함수를 전달인자로 받을 수 있고, 함수를 리턴할 수 있는 함수이다. 자바스크립트를 사용하며 자연스럽게 사용하던 개념이지만 더 자세히 알아보며 특징을 파악할 필요가 있다.

일급객체를 통해 알아봤듯이 함수를 변수에 할당하거나 함수를 리턴하는 것도 가능한데 **고차함수는 함수 자체를 리턴한다.** 변수에 할당 후 리턴하는 방식이 아닌 함수자체를 리턴하는 차이가 있을 뿐 동일하게 동작한다.

이때 다른함수(caller)의 전달인자로 전달되는 함수를 콜백함수(callback function)라고 하면 어떠한 작업이 완료되었을 때 호출하는 경우가 많아, 답신 전화를 받는다는 의미의 콜백함수라는 이름이 붙여졌다.

콜백함수를 전달받은 고차함수(caller)는 함수를 원하는 방식으로 호출할 수 있다.

함수를 리턴하는 함수는 하스켈 커리(Haskell Curry)의 이름을 따 `커링함수` 라고 하며, 커링함수를 따로 사용하는 경우에는 고차함수를 **함수를 전달인자로 받는 함수**에만 한정해서 사용하기도 한다. 단, 정확히 구분하면 고차함수에 커링함수가 포함된다는 것은 알아두자.

## 내장 고차함수

자바스크립트 내에는 기본적으로 제공하는 내장된 고차함수가 존재한다. 배열에서 사용하는 일부 메서드가 대표적인 고차함수에 해당한다. (filter, map, reduce, forEach, find, sort, some, every … ) 각 내장 고차함수의 특징과 사용법을 알아보자.

### filter()

---

`filter()` 메서드는 모든 배열의 요소 중 특정 조건을 만족하는 요소를 걸러내는 메서드이다.

```jsx
// 아래 코드에서 '짝수'와 '길이 5 이하'는 문법 오류(syntax error)에 해당합니다.
// 의미만 이해해도 충분합니다.
let arr = [1, 2, 3, 4];
let output = arr.filter(짝수);
console.log(output); // ->> [2, 4]

arr = ['hello', 'code', 'states', 'happy', 'hacking'];
output = arr.filter(길이 5 이하)
console.log(output); // ->> ['hello', 'code', 'happy']
```

위 예시에서 조건으로 전달되는 인자는 함수의 형태로 `filter()` 메서드는 고차함수에 해당한다. 조금 더 디테일하게 동작을 알아보면 다음과 같다.

```jsx
// 아래 코드는 정확한 표현 방식은 아닙니다.
// 의미만 이해해도 충분합니다.

let arr = [1, 2, 3];
// 배열의 filter 메서드는 함수를 전달인자로 받는 고차 함수입니다.
// arr.filter를 실행하면 내부적으로 arr에 접근할 수 있다고 생각해도 됩니다.
arr.filter = function (arr, func) {
  const newArr = [];
  for (let i = 0; i < arr.length; i++) {
    // filter에 전달인자로 전달된 콜백 함수는 arr의 각 요소를 전달받아 호출됩니다.
    // 콜백 함수가 true를 리턴하는 경우에만 새로운 배열에 추가됩니다.
    if (func(arr[i]) === true) {
      newArr.push(this[i]);
    }
  }
  // 콜백 함수의 결과가 true인 요소들만 저장된 배열을 리턴합니다.
  return newArr;
};

/*
 * filter 메서드의 보다 정확한 정의는 아래와 같습니다. 아래 코드를 이해하기 위해서는 다음 유닛에서 프로토타입과 this에 대한 학습이 필요합니다.
 * Array.prototype.filter = function(func) {
 *   const arr = this;
 *   const newArr = []
 *   for(let i = 0; i < arr.length; i++) {
 *     if (func(arr[i]) === true) {
 *       newArr.push(this[i])
 *     }
 *   }
 *   return newArr;
 * }
 */
```

`filter` 메서드는 배열의 요소를 콜백함수에 배열의 각 요소를 전달한다. 콜백함수는 전달받은 배열의 요소를 받아 함수를 실행하고, 조건에 따라 Boolean 타입을 반환한다.

### map()

---

`map()` 메서드는 `filter()` 와는 조금 달리 조건이 아닌 콜백 함수 내에서 선언된 동작을 배열의 각 요소에 적용해 새로운 배열을 반환하는 메서드이다. 

```jsx
// 만화책 모음
const cartoons = [
  {
    id: 1,
    bookType: 'cartoon',
    title: '식객',
    subtitle: '어머니의 쌀',
    createdAt: '2003-09-09',
    genre: '요리',
    artist: '허영만',
    averageScore: 9.66,
  },
  {
    id: 2,
    // .. 이하 생략
  },
  // ... 이하 생략
]; 

// 만화책 한 권의 부제를 리턴하는 로직(함수)
const findSubtitle = function (cartoon) {
  return cartoon.subtitle;
}; 

// 각 책의 부제 모음 
const subtitles = cartoons.map(findSubtitle); // ['어머니의 쌀', ...]
```

### reduce()

---

`reduce()` 는 얼핏 `map()` 과 비슷하지만 누적과 초기값이라는 특징을 가지고있다. 입력받은 배열의 콜백 내부의 행동에 따라 하나의 값으로 응축해 반환하며 초기값을 지정하지 않을 경우 배열의 첫 번째 요소가 초기 값으로 할당되어 동작한다.

```jsx
// 단행본 모음
const cartoons = [
  {
    id: 1,
    bookType: 'cartoon',
    title: '식객',
    subtitle: '어머니의 쌀',
    createdAt: '2003-09-09',
    genre: '요리',
    artist: '허영만',
    averageScore: 9.66,
  },
  {
    id: 2,
    // .. 이하 생략
  },
  // ... 이하 생략
];

// 단행본 한 권의 평점을 누적값에 더한다.
const scoreReducer = function (sum, cartoon) {
  return sum + cartoon.averageScore;
}; 

// 초기값에 0을 주고, 숫자의 형태로 평점을 누적한다.
let initialValue = 0 
// 모든 책의 평점을 누적한 평균을 구한다.
const cartoonsAvgScore = cartoons.reduce(scoreReducer, initialValue) / cartoons.length;
```

`reduce()` 는 요소의 합이나 숫자를 활용하는 누적에만 사용하는 것이 아니라 문자열, 포함여부 등 콜백의 내용에 따라 다양한 활용도를 가진다.

```jsx
function joinName(resultStr, user) {
  resultStr = resultStr + user.name + ', ';
  return resultStr;
}

let users = [
  { name: 'Tim', age: 40 },
  { name: 'Satya', age: 30 },
  { name: 'Sundar', age: 50 }
];

users.reduce(joinName, '');
```

## 고차함수를 쓰는이유?

---

컴퓨터 공학의 근간을 이루는 개념중 특히 추상화를 알아보며 고차함수에 대해 좀 더 자세히 알아보자. 추상화는 **복잡한 것을 압축해 핵심만 추출한 상태**를 말한다. 개인적으로 추상화를 이해할 때 어떠한 행동이나 개념에 대해 디테일한 요소가 아닌 중요하고 공통적인 속성에 중점을 두어 표현하는 것이라고 이해했는데, 추상화라는 말 자체가 추상적 개념이기에 각자마다 이해하는 형태가 조금씩 다를 수 있지만 `공통과 핵심` 에 초점을 두는 것이 중요하다 생각한다.

일상에서 사용하는 대부분, 사실 모든 것은 추상화의 결과로 볼 수 있는데 자동차의 엑셀, 교통카드, 메신저 등 대부분의 것은 추상화의 결과이다.

자바스크립트를 포함한 여러 프로그래밍 언어들 또한 추상화의 결과물로 브라우저의 엔진 또한 마찬가지 이다.

지금은 추상화에 대해 그리 깊이 알아야할 단계는 아니라고 생각하기에 추상화를 사용하는 이유에 대해

- 추상화 = 생산성(productivity)의 향상

이라는 것을 알아두고 이후 자세히 알아보자.

 

프로그램을 작성할 때 반복되는 로직을 함수로 작성하기도 하는데 이 또한 추상화의 좋은 예시로 볼 수 있다. 이러한 함수를 통해 얻은 추상화를 한 단계 더 높인 것이 고차함수이다.

추상화에 대한 여러 글들을 보면 이에 대해 이해할 수 있는 여러 표현들이 있었다.

- **소프트웨어 개발관점에서 추상화란 인터페이스에 의존하고, 구체적인 구현에는 의존하지 않는다.**
- **보통 함수를 기본적인 추상화 방법으로 사용한다.**
- **함수를 작게 만드는 것이 핵심이며, 함수가 하는 일도 하나여야 한다. 그 하나의 역할이 함수의 이름으로 표현되며, 이름만 가지고 무슨 역할을 하는지 명확히 파악되어야 한다.
함수가 커진다 -> 추상화를 제대로 하지 않은 것이다.**
- **함수내 블록화된 코드를 묶어서 새로운 함수로 만든다. -> 그 블록에 대한 지식을 대표적으로 표현하기 위한 추상화**
- **파일의 이름이나 디렉토리의 이름도 추상화의 일부이다.**

위 글 들을 읽으며 곰곰히 생각해보면 지금까지 사용하던 함수나 폴더 등도 추상화의 예시로 볼 수 있듯이 추상화는 표현의 방식이자 메커니즘이라고 생각할 수 있다.

---


## 마무리
가끔 사용하긴 했지만 효율성, 추상화 등 크게 관심을 가지지 않고 사용했던 것 같다. 이번 기회에 사용법, 활용 예시도 더 알아보고 개념도 보다 깊이 알아볼 필요가 있다고 생각했다.

실제 구현에서는 시간이나 상황에 쫓겨 활용방안을 생각하지 못했지만 고차함수를 사용하는 방식도 고려하고 구현을 해봐야한다.


<br><br><br>

<!-- ## 참고 사이트
- [태기의 개발 Blog](https://ljtaek2.tistory.com/140) -->
