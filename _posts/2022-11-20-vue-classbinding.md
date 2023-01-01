---

layout: post

title: Vue - ClassBinding

subtitle: TIL

categories: coding

tags: [Vue, Study]

---

# Vue ClassBinding

<aside>
💡 드롭다운, 반응형, 인터렉션 등 개발 시 class를 동적으로 변경해가며 사용해야 할 때가 있다.

</aside>

## 문법

```jsx
:class = "{'className': 'binding' }"
```

- 위와 같이 지정할 경우 true일 때 class 이름이 바인딩 되고, false일 때 class 이름이 제거된다.

```jsx
<template>
  <div>
    <h1 :class="{ red: isBind }">Class Bind</h1>
    <button @click="setBind">바인딩</button>
  </div>
</template>

<script>
export default {
  data() {
    return {
      isBind: false,
    };
  },
  methods: {
    setBind() {
      this.isBind = !this.isBind;
    },
  },
};
</script>

<style scoped>
.red {
  color: red;
}
</style>
```

- 위 코드에서 button을 클릭해 `setBind` 가 실행되어 isBind가 변경될 겨우 h1에 지정된 class 바인딩 또한 바뀌며 스타일이 적용된다.

```jsx
<template>
  <div>
    <div :class="{ text_red: true }">Class Binding</div>
  </div>
</template>

<script>
export default {
  data() {
    return {}
  }
}
</script>

<style scoped>
.active {
  color: violet;
  font-weight: bold;
}

.text-red {
  color: red;
}
</style>
```

- 즉, Class 바인딩은 해당 name의 참 거짓으로 결정되어 바인딩 된다.

```jsx
<template>
  <div>
    <div :class="{ text_red: hasError, active: isActive }">Class Binding1</div>
    <div :class="listClass">Class Binding2</div>
    <button @click="activeClickHandler">active</button>
  </div>
</template>

<script>
export default {
  data() {
    return {
      isActive: false,
      hasError: true,
      listClass: ['active', 'text_red']
    }
  },
  methods: {
    activeClickHandler() {
      this.isActive = !this.isActive
      this.hasError = !this.hasError
    }
  }
}
</script>

<style scoped>
.active {
  color: violet;
  font-weight: bold;
}

.text_red {
  color: red;
}
</style>
```

- 간단하게 버튼을 활용하여 스타일을 변경하는 class binding을 구현하였다.
- 흔하게 사용하지 않지만 배열 자체를 Class로 바인딩하는 방식도 가능하다.

## 반복문에서의 조건 Class 바인딩

```jsx
<template>
  <div>
    <h1
      v-for="item in forDatas"
      :key="item.id"
      :class="{ red: item.id === '4' }"
    >
      {{ item.name }}
    </h1>
  </div>
</template>

<script>
export default {
  data() {
    return {
      forDatas: [
        { id: "1", name: "super" },
        { id: "2", name: "pil" },
        { id: "3", name: "hello" },
        { id: "4", name: "world" },
      ],
    };
  },
};
</script>

<style scoped>
.red {
  color: red;
}
</style>
```

- `forDatas` 를 반복하며 id 값이 4일 경우 클래스를 바인딩 한다.
- 조건의 결과 값이 true면 바인딩 되기 때문에 여러 조건식이 사용 가능하다.

## if문을 활용한 Class 바인딩

```jsx
<template>
  <div>
    <h1 :class="[textColor === 'pinkText' ? 'pink' : 'blue']">Super Pil</h1>
    <button @click="changeColor">색상변경</button>
  </div>
</template>

<script>
export default {
  data() {
    return {
      textColor: "pinkText",
    };
  },
  methods: {
    changeColor() {
      if (this.textColor === "pinkText") {
        this.textColor = "blueText";
      } else {
        this.textColor = "pinkText";
      }
    },
  },
};
</script>

<style scoped>
.pink {
  color: pink;
}
.blue {
  color: blue;
}
</style>
```

- 삼항연산을 활용해 textColor 값에 따라 다른 Class를 바인딩 할 수 있다.

## 다수의 Class 바인딩

```jsx
<template>
  <div>
    <p :class="[{ blue: isBlue }, { bold: isBold }]">Super Pil</p>
    <button @click="changeColor">색상변경</button>
    <button @click="changeBold">Bold변경</button>
  </div>
</template>

<script>
export default {
  data() {
    return {
      isBlue: false,
      isBold: false,
    };
  },
  methods: {
    changeColor() {
      this.isBlue = !this.isBlue;
    },
    changeBold() {
      this.isBold = !this.isBold;
    },
  },
};
</script>

<style scoped>
.blue {
  color: blue;
}
.bold {
  font-weight: bold;
}
</style>
```

- 각기 다른 조건을 사용해 하나의 태그 내에 여러 Class를 바인딩 할 수 있다.
