---

layout: post

title: Vue - Data Binding 01

subtitle: TIL

categories: coding

tags: [Vue, Study]

---

# Vue Data Binding 01

## data

<aside>
💡 `data` 는 객체 혹은 함수의 형태로 vue instance가 사용할 데이터를 가지고있는 속성이다. 각 태그에 정적으로 데이터를 넣고 페이지를 렌더링 하게되면 사실상 spa를 사용하는 이유가 없을 것이다. 페이지 내 요소들을 동적으로 관리, 사용하기 위해 data를 사용한다.

</aside>

```jsx
<script>
export default {
  data() {
    return {
      userName: 'Bloxxom'
    }
  }
}
</script>
```

- 객체와 함수 형태 모두 사용할 수 있지만 컴포넌트 내에서 사용할 경우 함수 내에서 데이터를 반환해야한다. 만일 동일 컴포넌트가 여러번 반복될 경우 객체 형식은 동일하게 참조되어 각각의 컴포넌트 별 데이터 변환이 불가능하다. 하지만 함수의 경우 호출될 때마다 각기 다른 객체를 반환하게되고 각각의 독립적인 객체를 참조하게 되어 개별적으로 동작이 가능해진다.
- 만약 위 코드의 `userName` 을 변수로 빼내어 사용하면 역시 동일한 포인터를 가리켜 의도치 않게 동작할 수 있으니 함수 내부에서 return 할 수 있도록 지정하자.

```jsx
<script>
export default {
  data() {
    return {
      userName: 'Bloxxom',
			arr: [],
			obj: {}
    }
  }
}
</script>
```

- 문자열 외에도 배열, 객체 등 대부분의 타입을 모두 사용할 수 있으니 필요에 따라 선택해서 사용하자.

### 데이터 사용

```jsx
<template>
  <div>
    <h1>{{ userName }}</h1>
    <p></p>
  </div>
</template>
```

- data에 지정한 데이터를 사용하고 싶을 땐 `보간법` 이라 불리는 문법을 사용하면 된다. react와 비슷하면서 한번더 감싸주는 방식이기 때문에 조금은 주의를 기울여 사용하자.

## Methods

<aside>
💡 data가 사용할 데이터를 담고있는 속성이라면 `methods`는 vue instance가 사용할 함수를 담고있는 속성이다.

</aside>

```jsx
<script>
export default {
  data() {
    return {
      userName: 'Bloxxom'
    }
  },
  methods: {
    clickEventHandler: () => {
      return 'hello!'
    }
  }
}
</script>
```

- 위 처럼 컴포넌트 내부에서 사용할 함수를 지정해 사용할 수 있다. 단, this를 사용할 땐 Arrow function 사용에 주의하자.

```jsx
<template>
  <div>
    <h1>{{ userName }}</h1>
    <p>{{ clickEventHandler() }}</p>
  </div>
</template>
```

- data와 비슷하게 method 또한 컴포넌트 태그 내에서 사용이 가능하다. 위 예제와 달리 이벤트 핸들러의 경우 실행시점을 생각해야한다.

### this와 Arrow function

<aside>
💡 JS 에서 this는 굉장히 자유로운 친구다. arrow function을 사용하다 보면 this가 window 혹은 undefined를 가리키는 경우가 많은데 arrow function은 this의 위치를 변환(?) 시키기 때문이다. 호출하는 대상에 따라 변하긴 하지만 일단 가볍게 이정도로 알아두고 가자.

</aside>

```jsx
<script>
export default {
  data() {
    return {
      userName: 'Bloxxom'
    }
  },
  methods: {
    clickEventHandler: () => {
      return this.userName
    }
  }
}
</script>
```

- 만약 위처럼 arrow function 내부에서 this를 사용하면 의도와 달리 vue instance를 가리키지 않아 this 데이터를 가져오지 못한다.

```jsx
<script>
export default {
  data() {
    return {
      userName: 'Bloxxom'
    }
  },
  methods: {
    clickEventHandler: function(){
      return this.userName
    }
  }
}
</script>
```

- 따라서 의도적으로 this를 사용할 수 있는 리터럴 형식을 사용하면 된다. 콜백 함수와 사용 의도에 따라 선택적으로 사용할 필요가 있다.

## HTML Binding

<aside>
💡 컴포넌트를 페이지에 구성요소로 사용하며 각 html을 생성하는 방식도 있지만 때에 따라 데이터 자체에서 html을 구성해 내보내야하는 경우도 있다. 이러한 경우 vue에서 처리하는 방식을 알아보자.

</aside>

```jsx
<template>
  <div>
    <div>{{ htmlString }}</div>
  </div>
</template>
<script>
export default {
  name: 'bindingHtml',
  components: {},
  data() {
    return {
      htmlString: '<p style="color: red;">html binding</p>'
    }
  },
  setup() {},
  created() {},
  mounted() {},
  unmounted() {},
  methods: {}
}
</script>
```

- 위와 같이 data 바인딩을 사용하는 방식으로 처리하면 해당 문자열 자체가 태그에 나타난다.

```jsx
<template>
  <div>
    <div>{{ htmlString }}</div>
    <div v-html="htmlString"></div>
  </div>
</template>
<script>
export default {
  name: 'bindingHtml',
  components: {},
  data() {
    return {
      htmlString: '<p style="color: red;">html binding</p>'
    }
  },
  setup() {},
  created() {},
  mounted() {},
  unmounted() {},
  methods: {}
}
</script>
```

- 따라서 `v-html` 을 사용하여 태그 속성으로 지정한다면 해당 태그 내부에 html String을 DOM 요소로서 사용할 수 있다.

