---

layout: post

title: Big O - 공간복잡도

subtitle: Udemy class

categories: algorithm

tags: [javascript, algorithm]

---

# 공간 복잡도란?

공간 복잡도는 알고리즘에서 사용하는 메모리의 양을 나타낸다.

공간 복잡도는 보조공간(Auxiliary Space)과 입력공간(input size)을 합친 포괄적인 개념이다. 여기서 보조 공간(Auxiliary Space)은 알고리즘이 실행되는 동안 사용하는 임시공간을 뜻하기 때문에 입력공간(input size)을 고려하지 않는다.

즉, 공간 복잡도는 입력되는 것을 제외한 알고리즘 자체가 필요로 하는 공간을 의미한다.

시간 복잡도에서는 입력이 커질수록 알고리즘의 실행 속도가 어떻게 바뀌는지 분석했다. 이번엔 입력이 커질수록 알고리즘이 얼마나 많은 공간을 차지하는지에 대해 분석하는 공간복잡도에 대해 알아보자.

### 공간 복잡도의 룰

- `Boolean` , `Number` , `undefined` , `null` 은 자바스크립트에서 모두 불변 공간이다. (숫자가 1이던 1000이던 모두 불변 공간이라 여긴다.)
- String(문자열)은 O(n) 공간이 필요하다. (n 이 문자열의 길이일 경우 길이에 따라 공간을 차지하기 때문)
- Reference Type 즉, 배열과 객체도 대부분 O(n)이라고 여긴다. (n 은 배열의 길이거나 객체 key의 갯수일 수 있다.)

### 예시 1.

![Untitled](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/772d150b-99ce-4d2f-a67a-f679f2dfa4ac/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45EIPT3X45%2F20220513%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20220513T200502Z&X-Amz-Expires=86400&X-Amz-Signature=423175d9b5f934a6e8cddf5d2d9c0f3994e35cf8d10d0d1d23fb76839cde2faf&X-Amz-SignedHeaders=host&response-content-disposition=filename%20%3D%22Untitled.png%22&x-id=GetObject)

위 예시를 보면 배열 `arr` 을 받아 그 길이만큼 반복하며 반복횟수를 `total` 에 누적하여 더하는 코드이다. 이코드의 공간 복잡도를 살펴보기 전 확실하게 해야할 부분은 이 파트에서 기존의 시간복잡도가 아닌 공간복잡도 이므로 우리는 **공간**에 집중해야한다.

![Untitled](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/934e9204-4be1-49fd-a174-d75c2ce15967/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45EIPT3X45%2F20220513%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20220513T200513Z&X-Amz-Expires=86400&X-Amz-Signature=1e9ad001a99d5d498fcacab8d3170a4df5348015de8cca2265650f0510ac1376&X-Amz-SignedHeaders=host&response-content-disposition=filename%20%3D%22Untitled.png%22&x-id=GetObject)

각 변수를 살펴보면 원시타입 `number` 이며 불변의 공간이기 때문에 입력의 크기가 차지하는 공간과는 아무 상관이 없다.

![Untitled](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/530f4f30-b8c0-406c-bf67-8411fc7d8776/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45EIPT3X45%2F20220513%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20220513T200527Z&X-Amz-Expires=86400&X-Amz-Signature=9e3425127425e2f2fc9644defcb8810c8894415973ad829d7d334b8b0a8c1193&X-Amz-SignedHeaders=host&response-content-disposition=filename%20%3D%22Untitled.png%22&x-id=GetObject)

그렇다면 이러한 경우는 어떨까? 
이전과 달리 새로운 배열을 선언하고 반복과정에서 배열에 값을 추가하고 있다.

![Untitled](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/eb1ce52d-e2bc-4fa0-bae6-30018f71533e/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45EIPT3X45%2F20220513%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20220513T200537Z&X-Amz-Expires=86400&X-Amz-Signature=b9ccc94715ef8bc3ff6e84b5d9af806c293eeecc6bcfe53d86a837e6ff0d6f11&X-Amz-SignedHeaders=host&response-content-disposition=filename%20%3D%22Untitled.png%22&x-id=GetObject)

따라서 `arr` 즉, input이 커짐에 따라 배열의 공간도 그와 비례해서 커지게 되므로 `O(n) space` 공간을 차지한다.

## 로그(log)함수에 대해

지금까지 알고리즘의 시간, 공간 복잡도를 알아보며 `O(n), O(1), O(n^2)` 등 비교적 간단한 복잡도를 예시로 보았지만 보다 간단하지 않은 복잡도를 가진 경우가 있다.

흔한 복잡도의 경우 이해나 파악이 꽤나 쉽지만 여러 알고리즘을 만나다 보면 더 어렵거나 흔하지 않은 수학 개념의 복잡도가 포함될 수 있으므로 그 중에서도 기본적인 `log` 에 대해 알아보자.

### 로그(log) ?

우선 로그에 대해 가볍게 알아보자면 로그함수는 지수함수의 역함이다. 나눗셈과 곱셈이 짝인듯이 로그함수와 지수함수는 짝이다.

```jsx
log2(8) = 3
```

이 예시를 보면 `log2(8)` 이 `3` 과 같다는 수식이 된다. 따라서 이 수식이 하는 질문은 `2의 몇승을 한 값이 8이 되는가?` 를 물어보는 것이다. 

따라서 밑과 같은 수식으로 표현할 수 있다.

```jsx
2^3 = 8
```

![Untitled](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/5d5a6d0b-0fb1-4b2f-9b54-5b067d2ce2e0/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45EIPT3X45%2F20220513%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20220513T200552Z&X-Amz-Expires=86400&X-Amz-Signature=ac7cca90b98018c8d5596d46e62abe4f957a423bb34e8f90a6d11b07b85b1027&X-Amz-SignedHeaders=host&response-content-disposition=filename%20%3D%22Untitled.png%22&x-id=GetObject)

위에서 설명했듯이 지수함수와 역함의 관계로 마치 `1 / 2` 가 0.5 이고 반대로 `2 * 0.5` 가 1인 듯이 로그함수와 지수함수는 짝의 관계라 할 수 있다.

참고하고 있는 강의나 다른 글들을 살펴보면 알고리즘에서 로그의 지수를 생략하여 표기하는 경우가 있는데, 이는 기존의 Big O 표기법과 같이 log의 지수가 아닌 그래프의 상수시간, 추세를 로그시간과 비교한다면 로그 뒤에 붙은 숫자는 크게 상관하지 않기 때문이다. 언제나 **추세** 를 중요하게 여겨야한다.

단, `log` 는 자체로는 계산이 이뤄지지 않으므로 실제 계산을 진행하거나 말로 표현할 때는 지수를 붙여야 한다.

### 간단한 규칙

어떠한 이진 로그 `log2()` 를 대략 계산하기 위해서는 그 숫자가 1보다 작아지기 전에 2로 나눠지는 횟수이다.  

![Untitled](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/1c993716-6c19-4a1e-8f7e-4be32cefaeb0/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45EIPT3X45%2F20220513%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20220513T200606Z&X-Amz-Expires=86400&X-Amz-Signature=8aed5de688d04ff4acf4b6387643a86f518f3174e3a52820877db5f4e1ba6241&X-Amz-SignedHeaders=host&response-content-disposition=filename%20%3D%22Untitled.png%22&x-id=GetObject)

위 예시를 보면 확실하게 알 수 있지만 25처럼 정확하게 나눌 방법이 없는 경우도 있다. 하지만 복잡도에 한해서 실제 계산은 그리 중요하지 않다.

꾸준히 이야기 하지만 **전반적인 추세** 즉, 그래프에 어떻게 보이는가? 가 가장 중요하다.

![Untitled](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/a023d325-faf0-4068-89b2-ebed3405de3b/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45EIPT3X45%2F20220513%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20220513T200615Z&X-Amz-Expires=86400&X-Amz-Signature=a3b68a7df471c088235c1ad58c2216b497b5734dfafc9134e657594ec99b4a3d&X-Amz-SignedHeaders=host&response-content-disposition=filename%20%3D%22Untitled.png%22&x-id=GetObject)

`log n` 시간 복잡도를 가진 알고리즘은 보다시피 처음에는 조금 가파르지만 서서히 경사가 작아지며 `O(1)` 의 그래프와 가까이 붙어있다. 따라서 `log n` 의 시간 복잡도를 가진 알고리즘은 시간 성능이 매우 효율적이라고 볼 수 있다.

이 부분을 보며 개인적으로도 수학을 접한 경험이 별로 없어 처음에는 막막했지만 여러 자료를 검색하고 천천히 이해하며 진행하니 꽤나 재밌고 생각만큼 딱딱한 학문이 아니라고 생각된다. 만약 이 글을 보며 이해가 되지 않는다면 적어도 어떠한 복잡도를 가진 알고리즘이 비교적 효율적인가를 따질 수 있는 이해도만 생긴다면 점차 이해할 수 있는 범위가 넓어질 것이다.

### 어디에 사용될까?

- 몇몇의 탐색 알고리즘들은 로그 시간 복잡도를 가지고 있다. 이는 추후 알고리즘 학습을 진행하며 더 알아보자.
- 효율적인 정렬 알고리즘들도 로그와 관련되어있다. 모든 정렬이 그런것은 아니지만, 효율적인 정렬 알고리즘 들은 로그와 관련이 있다.
- 재귀와 관련이 있다. 단, 재귀에서는 시간이 아닌 공간 복잡도와 관련이 있으며, 어쩌면 가장 중요할 수 있는 내용이지만 추후 재귀를 다루면 알아보자.

### 요약

- 알고리즘의 성능을 측정하기 위해 `Big O` 표기법을 사용한다. 이는 입력의 크기가 늘어날수록 전체적인 추세가 어떻게 변하는가에 대한 추세와 관련이 있다.
- 결국 실행 시간과 공간의 복잡도가 어떻게 변하는지가 궁금할 것이다. 따라서 `Big O` 를 통해 시간과 공간 복잡도에 대한 이해를 높일 수 있다.
- 실제 계산의 정확도가 아닌 전체적인 추세를 중요하게 여긴다.
- `Big O` 로 측정되는 알고리즘의 시간과 공간 복잡도는 하드웨어의 영향을 받지 않는다. 따라서 어떠한 환경에서, 어떠한 기기로 실행하는지는 중요하지 않으며 실행될 연산의 갯수를 따져 전반적인 추세와 차이는 크지 않을 것이다.
- `Big O` 는 세상 모든곳에서 사용된다.




---


## 마무리

수학적인 내용들도 함께 공부하다보니 생각보다 시간이 좀 걸렸지만 시간을 들여서 이해를 할 수 있었기에 만족했다. 

예시에서 보여준 복잡도들은 대부분 파악하기 쉬웠지만 이후 학습을 진행하며 더 많고 복잡한 알고리즘을 만날 것이기에 이외에도 수학, Big O에 대한 디테일한 학습이 더 필요할 것 같다.

<br><br><br>

## 참고 사이트
- [Poiemaweb](https://poiemaweb.com/)
