# Vue - Computed

<aside>
💡 template 내에 데이터를 바인딩 하며 로직을 처리할 수 있지만 코드가 길어질 수록 가독성은 떨어진다. 따라서 template에 사용할 복잡한 로직은 `computed` 로 처리해 깔끔하게 사용하자.

</aside>

```jsx
<div id="example">
  {{ reversedMessage }}
</div>

var vm = new Vue({
  el: '#example',
  data: {
    message: 'Hello'
  },
  computed: {
    // 산출 getter 함수
    reversedMessage: function () {
      // `this`는 「vm」 인스턴스를 뜻한다
      return this.message.split('').reverse().join('')
    }
  }
})
```

- `computed` 를 활용해 method를 생성하고 간단하게 사용하는 방식이다. 재 사용성이 높아지고 template 코드는 보다 깔끔해졌다.

## Setter 함수

<aside>
💡 `computed` 는 기본적으로 getter 함수만 존재하지만 필요에 의해 setter를 설정할 수 있다.

</aside>

```jsx
computed: {
  fullName: {
    // getter 함수
    get() {
      return this.firstName + ' ' + this.lastName
    },
    // setter 함수
    set(newValue) {
      const names = newValue.split(' ')
      this.firstName = names[0]
      this.lastName = names[names.length - 1]
    }
  }
}
```

## computed

<aside>
💡 method와 computed 모두 같은 역할을 할 수 있지만 엄연히 사용할 땐 구분이 필요하다.

</aside>

```jsx
methods: {
  reverseMessage: function () {
    return this.message.split('').reverse().join('')
  }
}

computed: {
  reversedMessage: function () {
    return this.message.split('').reverse().join('')
  }
}
```

### computed는 반응적인 의존관계에 의해 캐시화 된다.

- 위 코드에서 message가 변하지 않는 한 `reverseMessage` 가 몇번이고 호출되어도 함수는 재실행 되지 않고 이전 결과 값을 반환한다.

### method는 리렌더링 마다 함수를 재실행한다.

- computed와 달리 method는 상태의 변화로 인해 페이지가 리렌더링 될 때마다 함수를 재실행한다.

### Why chaching?

- 계산이 많이 걸리는 복잡한 로직의 경우 해당 로직이 계속해서 실행될 경우 시간이 점점 느려지게된다. 따라서 `computed` 를 사용하면 이전 결과 값을 그대로 반환하기 때문에 이러한 문제를 해결 가능하다.
- 반응적, 캐싱의 필요 유무에 따라 method와 computed를 구분하여 사용하자.

## computed Vs watch

<aside>
💡 watch는 react의 useEffect와 비슷한 녀석이다. 근데 생각해보면 반응적인 측면에서 computed도 비슷한데 두 기능의 차이를 알아보자.

</aside>

- 대부분의 경우 연산 프로퍼티 (computed)를 사용하는 것이 적절하지만 필요에 따라 watch를 사용해야하는 경우도 있다.

1. 데이터를 업데이트 할 때 비동기 처리, 혹은 비교적 무거운 처리를 실행할 경우
2. 감시할 데이터를 지정하고 해당 데이터가 바뀌면 선언한 함수를 실행하기 위한 명령형 프로그래밍이 필요한 경우

```jsx
// watch
  data: {
    firstName: 'Foo',
    lastName: 'Bar',
    fullName: 'Foo Bar'
  },
  watch: {
    firstName: function (val) {
      this.fullName = val + ' ' + this.lastName
    },
    lastName: function (val) {
      this.fullName = this.firstName + ' ' + val
    }
  }

//computed
  data: {
    firstName: 'Foo',
    lastName: 'Bar'
  },
  computed: {
    fullName: function () {
      return this.firstName + ' ' + this.lastName
    }
  }
```