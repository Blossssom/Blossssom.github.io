---

layout: post

title: 객체 사용법

subtitle: TIL codestates

categories: coding

tags: [javascript]

---
# 객체 사용법

<aside>
JS는 사실상 모든 것이 객체라 볼 수 있다. 그만큼 근본적으로 매우 중요한 부분이며 확실히 이해가 필요한 부분이다.

</aside>

## 객체선언

```jsx
let obj = {
	name: 'blossom',
	age: 28,
	print: function() {
		console.log(`${this.name}`);
	}
};

obj.print();
```

<aside>
위와 같은 리터럴 방식의 선언이 가장 일반적인 방식이며 객체 내부에는 함수, 또다른 객체 등 여러 방식이 들어갈 수 있다.

</aside>

```jsx
let name = 'blossom';
let age = 28;

let abj = {
	name,
	age,
	print: function() {
		console.log(`${this.name}`);
	}
};
```

<aside>
만약 이미 선언한 변수, 함수를 객체의 프로퍼티로 추가하고 싶다면 다음과 같이 이름만 넣어주면된다.

</aside>

```jsx
let obj = {};

obj.name = 'blossom';
obj.print = function() {
	console.log(`${this.name}`);
};
```

<aside>
또는 위와 같이 빈 객체 혹은 이미 선언된 객체의 값을 변경, 생성할 때는 다음과 같이 선언이 가능하다.

</aside>

## 프로퍼티 접근

```jsx
let obj = {
	name: 'blossom',
	age: 28,
	print: function() {
		console.log(`${this.name}`);
	}
};

console.log(obj.name);
console.log(obj['name']);
```

<aside>
객체 내부에 정의된 프로퍼티를 접근할 때는 다음과 같이 접근이 가능하다. 단, 함수의 경우 `[]` 로 접근할 수 없으니 `.` 로 접근하자

</aside>

```jsx
let obj = {
	name: 'blossom',
	age: 28,
	print: function() {
		console.log(`${this.name}`);
	}
};

delete obj.name;
```

<aside>
객체의 프로퍼티를 삭제할 때는 `delete` 함수를 사용하여 삭제할 수 있다.

</aside>

```jsx
let obj = {
	name: 'blossom',
	age: 28,
	print: function() {
		console.log(`${this.name}`);
	}
};

console.log('name' in obj); // true
console.log('gender' in obj); // false
```

<aside>
객체 내부에 해당 프로퍼티가 존재하는지 확인할 때는 `in` 혹은 `hasOwnProperty` 를 사용하면 논리 값으로 이를 알려준다.

</aside>

```jsx
let obj = {
	name: 'blossom',
	age: 28,
	print: function() {
		console.log(`${this.name}`);
	}
};

for(key in obj) {
	console.log(key); // name, age, print
	console.log(obj[key]); // blossom, 28, blossom
}

```

<aside>
객체의 프로퍼티에 순차적으로 접근할 때는 반복문을 활용하여 쉽게 접근할 수 있다.

</aside>

## 계산된 프로퍼티(Computed Property)

<aside>
객체의 프로퍼티를 지정하는 방법도 여러가지가 존재한다. 자세히 알아보자

</aside>

```jsx
let a = 'age';

const user = {
	name: 'blossom',
	[a]: 28
};

console.log(user) // name: blossom, age: 28
```

<aside>
위와 같이 이미 선언된 변수를 `[]` 로 감싸 프로퍼티의 key 값으로 선언할 경우 변수에 할당된 값을 불러와 프로퍼티 명으로 사용할 수 있다.

</aside>

```jsx
const user = {
	[1 + 4]: 5,
	['Cherry' + 'Blossom']: 'hi!'
}

console.log(user); // 5: 5, CherryBlossom: hi!
```

<aside>
또한 위와 같이 연산식을 활용하여 계산된 값을 할당하는 것도 가능하다. 이러한 프로퍼티를 계산된 프로퍼티라고 한다.

</aside>


---


## 마무리

우선 기본적인 객체의 사용법과 프로퍼티에 접근하는 방식을 알아보았다. 

객체는 자바스크립트의 근본이기도 하니 이후 글에서 추가적으로 더 심도있게 다룰 예정이다.

<br><br><br>

<!-- ## 참고 사이트
- [javascript 알고리즘 & 자료구조 마스터 클래스 (Udemy)](https://www.udemy.com/course/best-javascript-data-structures/learn/lecture/28559451#reviews)
 -->