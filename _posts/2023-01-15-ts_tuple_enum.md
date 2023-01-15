---

layout: post

title: Tuple & Enum 타입

subtitle: TIL

categories: coding

tags: [typescript]

---

---

# Tuple & Enum 타입

<aside>
💡 typescript는 기존의 js에서 지원하지 않던 타입도 지원한다.

</aside>

## Tuple타입 지정

```jsx
const obj: {
  name: string;
  age: number;
  hobbies: string[];
  role: [number, string];
} = {
  name: "bloxxom",
  age: 30,
  hobbies: ["hello", "world"],
  role: [1, "desc"],
};
```

- 튜플은 실제로 js에서는 일반적인 배열로 해석되지만 typescript 개발 도중 에러를 발생시켜 길이와 타입을 고정시킨다.

```jsx
obj.role.push('admin');
// Ok

obj.role = [];
// Error

obj.role[2] = 1;
// Error
```

- tuple 타입으로 지정해도 push는 예외적으로 동작해 배열에 추가된다. 조금은 아쉬운 점이지만 타입에 맞지 않는 값이 들어갈 일은 없을 듯 하다.
- 또한 지정된 갯수와 다르게 초기화를 하려하면 역시 Error를 발생시킨다.

## Enum타입 지정

<aside>
💡 Enum은 특정 값들의 집합을 의미하는 자료형이다. typescript에서는 숫자형, 문자형 Enum을 제공한다.

</aside>

### 숫자형 Enum

```jsx
enum Category {
  Coffee,
  Cookie,
  Juice
}

enum Category {
  Coffee = 1,
  Cookie,
  Juice
}
```

- 위와 같이 열거가 필요한 요소를 Enum으로 지정하여 만들면 각 순서에 따라 0 부터 label이 붙게된다.
- 만약 Enum의 요소에 초기 값을 줄 경우 초기 값을 기준으로 1씩 증가한 label이 부여된다.

```jsx
enum Role {
  ADMIN,
  READ_ONLY,
  AUTHOR,
}
```

- 각 계정의 권한 지정이나 카테고리 등 구별요소 혹은 개별적인 고유 label이 필요한 경우 유용하게 사용 가능하다.

### 문자형 Enum

```jsx
enum Keypad {
  Attack = "a",
  Move = "s",
  Skill = "w",
}
```

- 문자형 Enum은 숫자와 달리 명확한 순서나 패턴이 없기 때문에 각 요소에 초기 값을 지정해야한다.
- 불편한 듯 하지만 숫자와 다르게 불명확 하지 않고 항상 명확한 값이 나온다.

## 동작 특징

<aside>
💡 Enum은 런타임, 컴파일 시점에 특징이 있다.

</aside>

### 런타임 시점 특징

```jsx
enum Guitar {
  Fender,
  Gibson,
  Ibanez,
}

const getGuitar = (guitar: { Fender: number }) => {
  console.log(guitar.Fender);
};

getGuitar(Guitar);
```

- Enum은 런타임 시점에 객체로 존재하기 때문에 위와 같이 객체로서의 접근이 가능하다.

### 컴파일 시점 특징

```jsx
enum Guitar {
  SpongeBob,
  Fender,
  Gibson,
  Ibanez,
}

type guitarLevel = keyof typeof Guitar;

const levelOfGuitar = (key: guitarLevel, message: string) => {
  const level = Guitar[key];

  if (level > Guitar.SpongeBob) {
    console.log("Good", message);
  } else {
    console.log("get out", message);
  }
};

levelOfGuitar("SpongeBob", "guitar");
```

- 런타임 시점에서는 객체지만 `keyof` 를 사용할 땐 `keyof typeof` 를 사용하는 권이 권장된다.

## 리버스 맵핑

```jsx
enum Guitar {
  SpongeBob,
  Fender,
  Gibson,
  Ibanez,
}

let guitar = Guitar.Fender;
let guitarName = Guitar[2];
```

- 리버스 맵핑은 숫자형 Enum에만 사용 가능하며 Enum의 key, value로 서로를 얻어낼 수 있다.