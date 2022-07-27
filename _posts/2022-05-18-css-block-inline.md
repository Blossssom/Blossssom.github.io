---

layout: post

title: block과 inline 속성

subtitle: CSS Study

categories: coding

tags: [CSS]

---

# block, inline 속성

<aside>
display 프로퍼티는 layout 정의에 자주 사용되는 중요한 프로퍼티이다.

</aside>

![](/post-img/css-block01.png)

<aside>
모든 HTML 요소는 아무런 CSS를 적용하지 않아도 기본적으로 브라우저에 표현되는 default 표시값을 가진다. HTML 요소는 block 또는 inline 특성을 갖는다.

</aside>

> display 프로퍼티는 상속되지 않는다.
> 

## 1.1 block 레벨 요소

---

<aside>
`block` 특성을 가지는 요소(block 레벨 요소 또는 block 요소)는 아래와 같은 특징을 갖는다.

</aside>

- 항상 새로운 라인에서 시작한다.
- 화면 크기 전체의 가로폭을 차지한다.(width: 100%)
- width, height, margin, padding 프로퍼티 지정이 가능하다.
- block 레벨 요소 내에 inline 레벨 요소를 포함할 수 있다.
- block 레벨 요소 예
- div
- h1 ~ h6
- p
- ol
- ul
- li
- hr
- table
- form

```html
<!DOCTYPE html>
<html>
<head>
  <style>
    div:nth-of-type(1) {
      background-color: #FFA07A;
      padding: 20px;
    }
    div:nth-of-type(2) {
      background-color: #FF7F50;
      padding: 20px;
      width: 300px;
    }
  </style>
</head>
<body>
  <div>
    <h2>블록 레벨 요소</h2>
    <p>width, height 미지정 → width: 100%; height: auto;</p>
  </div>
  <div>
    <h2>블록 레벨 요소</h2>
    <p>width: 300px → width: 300px; height: auto;</p>
  </div>
</body>
</html>
```

![](/post-img/css-block02.png)

## 1.2 inline 레벨 요소

---

<aside>
`inline` 특성을 가지는 요소(inline 레벨 요소 또는 inline 요소)는 아래와 같은 특징을 갖는다.

</aside>

- 새로운 라인에서 시작하지 않으며 문장의 중간에 들어갈 수 있다. 즉, 줄을 바꾸지 않고 다른 요소와 함께 한 행에 위치한다.
- content의 너비만큼 가로폭을 차지한다.
- **width, height, margin-top, margin-bottom 프로퍼티를 지정할 수 없다.** 상, 하 여백은 inline-height로 지정한다.
- inline 레벨 요소 뒤에 공백(엔터, 스페이스 등)이 있는 경우, 정의하지 않은 space(4px)가 자동 지정된다.
- inline 레벨 요소 내에 block 레벨 요소를 포함할 수 없다. inline 레벨 요소는 일반적으로 block 레벨 요소에 포함되어 사용된다.
- inline 레벨 요소 예
- span
- a
- strong
- img
- br
- input
- select
- textarea
- button

```html
<!DOCTYPE html>
<html>
<head>
  <style>
    span {
      background-color: red;
      color: white;
      padding: 10px;
      /* width, height, margin-top, margin-bottom 프로퍼티를 지정할 수 없다. */
      /* width: 200px; */
      /* margin: 10px; */
      /* 상, 하 여백은 line-height로 지정한다. */
      /* line-height: 50px; */
    }
  </style>
</head>
<body>
  <h1>My <span>Important</span> Heading</h1>
  <span>Inline</span>
  <span>Inline</span><span>Inline</span>
</body>
</html>
```

![](/post-img/css-block03.png)

## 1.3 inline-block 레벨 요소

---

<aside>
block과 inline 레벨 요소의 특징을 모두 갖는다. **inline 레벨 요소와 같이 한 줄에 표현되면서 width, height, margin 프로퍼티를 모두 지정할 수 있다.**

</aside>

- 기본적으로 inline 레벨 요소와 흡사하게 줄을 바꾸지 않고 다른 요소와 함께 한 행에 위치시킬 수 있다.
- block 레벨 요소처럼 width, height, margin, padding 프로퍼티를 모두 정의할 수 있다. 상, 하 여백을 margin과 line-height 두가지 프로퍼티 모두를 통해 제어할 수 있다.
- content의 너비만큼 가로폭을 차지한다.
- inline-block 레벨 요소 뒤에 공백(엔터, 스페이스 등)이 있는 경우, 정의하지 않은 space(4px)가 자동 지정된다.

```html
<!DOCTYPE html>
<html>
<head>
  <style>
    .wrapper {
      font-size: 0; /*요소간 간격을 제거*/
    }
    .inline-block {
      display: inline-block;
      vertical-align: middle; /* inline-block 요소 수직 정렬 */
      border: 3px solid #73AD21;
      font-size: 16px;
    }
    .box1 {
      width: 300px;
      height: 70px;
    }
    .box2 {
      width: 300px;
      height: 150px;
    }
  </style>
</head>
<body>
  <div class="inline-block box1">inline-block height 70px</div>
  <div class="inline-block box2">inline-block height 150px</div>

  <div class="wrapper">
    <div class="inline-block box1">inline-block height 70px</div>
    <div class="inline-block box2">inline-block height 150px</div>
  </div>
</body>
</html>
```

![](/post-img/css-block04.png)

<aside>
float 대신 inline-block 을 사용한 가로 정렬 예를 보자

</aside>

```html
<!DOCTYPE html>
<html>
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <meta http-equiv="X-UA-Compatible" content="ie=edge">
  <title>Infinite looping Crousel Slider</title>
  <style>
    @import url(https://fonts.googleapis.com/css?family=Open+Sans:300,400);

    body {
      font-family: "Helvetica Neue", Helvetica, Arial, sans-serif;
      color: #58666e;
      background-color: #f0f3f4;
    }

    .carousel-view {
      /* 자식 컨텐츠 너비에 fit */
      display: inline-block;
      position: relative;
      margin: 0 auto;
      border: 1px dotted red;
    }

    .carousel-container {
      /* 자식 컨텐츠의 줄바꿈을 무시하여 1줄로 가로 정렬한다. */
      white-space: nowrap;
      /* inline-block 레벨 요소 뒤에 공백(엔터, 스페이스 등)이 있는 경우,
         정의하지 않은 space(4px)가 자동 지정된다. 이것을 회피하는 방법이다. */
      font-size: 0;
      margin: 0;
      padding: 0;
    }

    .carousel-item {
      display: inline-block;
      list-style: none;
      padding: 5px;
    }

    .carousel-item:last-child {
      margin-right: 0;
    }

    .carousel-control {
      position: absolute;
      top: 50%;
      transform: translateY(-50%);
      font-size: 2em;
      color: #fff;
      background-color: transparent;
      border-color: transparent;
      cursor: pointer;
      z-index: 99;
    }

    .carousel-control:focus {
      outline: none;
    }

    .carousel-control.prev {
      left: 0;
    }

    .carousel-control.next {
      right: 0;
    }
  </style>
</head>
<body>
  <div id="carousel" class="carousel-view">
    <button class="carousel-control prev">&laquo;</button>
    <ul class="carousel-container">
      <li class="carousel-item">
        <a href="#">
          <img src="http://via.placeholder.com/400x150/3498db/fff&text=1">
        </a>
      </li>
      <li class="carousel-item">
        <a href="#">
          <img src="http://via.placeholder.com/400x150/3498db/fff&text=2">
        </a>
      </li>
      <li class="carousel-item">
        <a href="#">
          <img src="http://via.placeholder.com/400x150/3498db/fff&text=3">
        </a>
      </li>
    </ul>
    <button class="carousel-control next">&raquo;</button>
  </div>
</body>
</html>
```

## 요약

> block
> 
- 가로 100%, 세로 0 의 default 값을 가진다.
- 크기 지정이 가능하다.
- 수직으로 쌓인다.
- margin, padding을 전방향으로 사용할 수 있다.
- 주로 layout을 잡는 용도로 사용

> inline
> 
- 가로 0, 세로 0 의 default 값을 가진다.
- 크기 지정이 불가하고 콘텐츠의 크기만큼 공간을 사용한다.
- 수평으로 쌓인다.
- html 내의 줄바꿈을 인식해 공간이 생긴다.
- margin, padding을 위, 아래로는 사용할 수 없다.
- 주로 text를 넣는 용도로 사용

---


## 마무리

개인적으로 CSS는 설계가 가장어려운 파트라고 생각한다. 단순히 보여지는 것을 짜는건 어떻게든 가능하겠지만 유지, 보수나 새로운 콘텐츠의 추가가 일어났을 때 알맞게 적용되는 CSS를 짜는 것은 설계단계 부터 단단하게 쌓아 올려야 할 것이다. 

자바스크립트도 매우 중요하지만 CSS와 HTML에 대한 설계와 패턴에 대한 공부도 게을리 할 수 없을 것이다.

<br><br><br>

## 참고 사이트
- [태기의 개발 Blog](https://ljtaek2.tistory.com/140)
