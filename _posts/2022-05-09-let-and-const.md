---
layout: post
title: 변수(let) 과 상수(const)를 알아보아요
subtitle: TIL codestates
categories: coding
tags: [javascript]
---

# 변수와 상수 ?
한번 쯤 프로그래밍을 접해봤다면 변수 혹은 상수에 대해 들어봤을 거에요 
변수와 상수 모두 어떤한 값을 할당하기 위해 사용하지만 둘 사이에는 매우 큰 차이가 있답니다.

## 변수 (let)
변수와 상수는 기본적으로 3단계를 거쳐 생성돼요 
>1. 선언
>2. 초기화
>3. 할당

기존에 사용하던 `var` 는 위 과정에서 선언과 초기화가 동시에 일어나 할당하기 전에 호출이 가능해 예상치 못한 에러를 일으키는 상황을 일으켰답니다.

하지만 `let` 의 경우 선언, 초기화, 할당이 각각 순차적으로 일어나 초기화가 실제 코드에 도달했을 때 일어나기 때문에 이러한 문제점을 해결할 수 있었어요.

기본적인 변수의 사용법은 다음과 같아요.
~~~javascript

let name = 'this is variable';
let hello;
hello = 'Hello!';

키워드 변수명 = 값;

~~~
각각 부분을 살펴보면 위와 같이 타입과 변수 명 그리고 변수에 대한 값을 작성하여 사용해요. 

`let` 으로 선언된 `name` 이라는 변수에 `this is variable` 이라는 문자를 할당했다고 한다면 간단하게 이해할 수 있겠죠?

---

## 상수 (const)
상수는 변수와 달리 재할당이 불가능한 변하지 않는 값을 지정하는 타입이에요.
그렇기 때문에 코드 상에서 선언과 동시에 반드시 값을 할당해줘야 하며 선언과 초기화, 할당이 전부 동시에 일어난답니다.

~~~javascript

const name = 'this is constant';

~~~

## TDZ (Temporal Dead Zone) 이란?
TDZ는 일시적인 사각지대 라는 뜻이에요.
이 사각지대는 스코프의 시작 지점부터 초기화 시작 지점까지의 구간을 의미해.

위에서 설명한 선언의 3단계를 좀 더 알아보며 살펴볼까요?

![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FCfdPQ%2FbtqFNFCCfWu%2FEBd8c7QUZLSChL2AVVaiyK%2Fimg.jpg)
 
- <span style="color:yellow">선언 단계 (Declaration phase)</span> : 변수를 실행 컨텍스트의 변수 객체에 등록하는 단계에요. 이 객체는 스코프가 참조하는 대상이 되어요.
- <span style="color:indigo">초기화 단계 (Initialization phase)</span> : 실행 컨텍스트에 존재하는 변수 객체에 선언 단계의 변수를 위한 메모리를 준비하는 단계에요. 이 단계에서는 아직 할당된 값이 없기 때문에 메모리에 undefined로 초기화 된답니다.
- <span style="color:orange">할당 단계 (Assignment phase)</span> : 사용자가 undefined로 초기화된 메모리에 실제 값을 할당하는 단계에요.

예전 자바스크립트 코드 혹은 글 들을 보면 `var` 라는 타입을 심심치 않게 볼 수 있어요. 위에서 설명했듯이 문제점이 있어 현재의 `let` 과 `const` 를 사용하고 있는데요, 좀 더 깊이 살펴보며 정확하게 알아보아요.

먼저 `var` 변수의 라이프 사이클을 먼저 알아보아요.

![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FcpN7ly%2FbtqFMImMjbi%2FXax5yU47pHIffGGf6BYetk%2Fimg.jpg)
 
 `var` 는 위 사진과 같은 라이프 사이클을 가지고 있어요.
 위처럼 변수 선언과 초기화 단계가 동시에 진행되는 것을 알 수 있어요.
 그래서 실행 컨텍스트 변수 객체의 변수를 등록하고 메모리를 undefined로 만들어 버리고 변수를 선언하기 전에 호출해도 undefined로 호출되는 `호이스팅` 이라는 현상이 발생한답니다.

![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2Fbrfyzo%2FbtqFMHahg20%2FO4al3vnYiNideb03m6xB60%2Fimg.jpg)

`let` 타입은 `var` 와 다르게 선언 단계와 초기화 단계가 분리되어서 진행돼요. 
그렇기 때문에 실행 컨텍스트에 변수를 등록했지만, 메모리가 할당되지 않아 참조에러(ReferenceError)가 발생하고 **호이스팅이 일어나지 않는다!** 라고 보이는 것 뿐이에요.

아까 설명했듯이 TDZ는 스코프의 시작 지점부터 초기화 시작 지점까지의 구간을 말해요.
즉, `let` 또한 선언전, 실행 컨텍스트 변수 객체에 등록이 되어 호이스팅 되지만, 이 TDZ 구간에 의해 메모리가 할당이 되지 않아 참조에러가 발생하는 것 이랍니다.

참고로 `function` 키워드 함수는 아래와 같이 동작해요!
![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FL4IXl%2FbtqFNPydemy%2FWqwtWgxdhHKZZ3xylTcJpK%2Fimg.webp)

선언의 3단계를 동시에 진행해 버리는 기기괴괴한 동작이지요??

## 마무리
익숙하지 않은 분, 동작원리에 대해 모른다면 자바스크립트의 가장 기본적인 부분이지만 TDZ 영역까지 살펴보니 이해하지 못하는 부분이 많죠?
저 또한 동작원리에 대해 자세히 알지 못해 조금씩 헷갈리는 부분이 많답니다.
변수를 사용할 때 마다 이유를 생각해 가며 다같이 차근차근 발전해 보아요 :hamster:
