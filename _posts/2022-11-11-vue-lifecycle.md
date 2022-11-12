---

layout: post

title: Vue - LifeCycle

subtitle: TIL

categories: coding

tags: [Vue, Study]

---

# Vue LifeCycle

<aside>
💡 기존 리액트를 사용하다 이제 vue를 사용해야하는 상황이다. 하나씩 정리하며 vue와 친해져보자.

</aside>

## createApp

```jsx
// 한번에 지정하기
Vue.createApp({ count }).mount(#app')

// 상수로 담아 지정
const app = Vue.createapp({})
```

- Vue의 오브젝트로 호출한 `createApp` 은 앱의 여러 옵션을 받으며 rootComponent를 구성하는데 사용된다.
- mount 메서드로 지정된 셀렉터는 렌더링의 범위를 나타내며 이를 사용해 앱 전체 혹은 앱의 일부를 vue에서 제어할 수 있다.

## LifeCycle

<aside>
💡 vue의 라이프사이클은 크게 4가지로 나뉜다.
- Creation
- Mounting
- Updating
- Destruction

</aside>

![Untitled](/post-img/lifecycle.png)

- 모든 lifecycle hook은 자동으로 this 컨텍스트가 인스턴스에 바인딩되어 있어 data, methods, computed 등의 속성에 접근할 수 있다.

## 1. Creation component 초기화

컴포넌트들을 초기화하는 단계로 해당 단계에서 실행되는 훅들이 라이프사이클 중에서 가장 처음 실행된다.

- 서버 렌더링에서 지원되는 단계
- 클라이언트 혹은 서버 렌더 단에서 처리해야할 작업이 해당 단계에서 진행
- Component 가 DOM에 추가되기 전으로 DOM에 접근할 수 없음

위에서 설명된 `new Vue()` 와 `init()` 으로 instance를 생성하고 Event와 LifeCycle을 초기화 하는 단계이다.

### beforeCreate

<aside>
💡 instance가 직전에 초기화 된 후 데이터 관찰 및 event / 감시자 설정 전에 동기적으로 호출

</aside>

- 모든 hook 중에서 가장 먼저 호출되는 hook으로 아직 `data` 와 `events` 가 세팅되지 않은 시점이기 때문에 접근 시 에러가 발생된다.

```jsx
export default {
  data() {
    return{
      content : ''
    }
  },
  
  beforeCreate(){
    //can't use Data(this.title ...), event
  }
}
```

### Created

- instance 작성 후 동기적으로 호출된다.
- 데이터 처리, 계산된 속성, method, 감시 / 이벤트 콜백 등과 같은 옵션 처리를 완료한다.
- 아직 `mount` 가 되지않은 시점으로 `$el` 속성은 아직 사용할 수 없다.

## 2. Mounting DOM 삽입 단계

<aside>
💡 Mounting 단계는 초기 렌더링 직전에 Component에 직접 접근할 수 있다. 서버렌더링에서 지원하지 않고 초기 렌더링 직전에 DOM을 변경하고자 한다면 해당 단계에서 활용할 수 있다.

</aside>

### BeforeMount

- `Mount` 가 실행되기 직전에 호출된다.
- `render` 함수의 첫 호출 시기이다.
- 대부분의 경우 사용을 권장하지 않는다.

### Mounted

- `el` 이 새로생성된 `vm.$el` 로 대체된 instance가 마운트 된 직후 호출
- Component, Template, 렌더링 된 DOM에 접근할 수 있다.

<aside>
💡 `$` 표시는 리액트에서는 흔히 볼 수 없어서 당황스러울 수 있다. 일반적인 JS 에서는 Jquery 변수를 강조할 때 사용하며 Vue에서는 **전역 객체 속성**을 의미한다. 

즉, private하게 사용하는 아닌 public 하게 사용하는 속성 (this.$emit, this.$router.push({})… ) 등을 의미한다.

또한 _ (underScore)는 JS와 Vue 모두 private한 속성을 의미한다.

</aside>

root instance 가 문서 내의 Element에 mount되어 있으면, `mounted` 가 호출될 때 `vm.$el` 도 문서 내에 있게된다.

- `mounted` 는 모든 자식 Component가 마운트 된 상태를 보장하지 않는다.
- `mounted` 내부에서 `vm.$nextTick` 를 사용하면 전체가 렌더링된 상태를 보장한다.

```jsx
export default {
  mounted(){
    console.log(this.$el.textContent) //can use $el
  this.$nextTick(function() { 
    //모든 화면이 렌더링된 후 실행합니다
    })
  }
}
```

<aside>
💡 부모와 자식 관계의 Component 에서 생각한 상태로 mounted가 발생하지 않는다.

</aside>


## 3. Updating

<aside>
💡 Component에서 사용되는 반응형 속성들이 변경되거나 어떠한 이유로 재렌더링 발생 시 실행된다. 디버깅 혹은 프로파일링 등을 위해 Component의 재 렌더링 시점을 알고 싶을 때 사용한다.

</aside>

### beforeUpdate

- Component의 데이터가 변경되어 업데이트 사이클이 시작될 때 실행된다 ( 정확히는 DOM이 패치되기 전에 데이터가 변경될 때 호출된다. )
- 업데이트 전 기존 DOM에 접근할 수 있다.
- 모든 자식 Component의 재 렌더링 상태를 보장하지 않는다.

전체화면이 재 렌더링 될 때가지 기다리려면 updated 내부에서 `vm.$nextTick` 을 사용한다.

```jsx
updated() {
  this.$nextTick(function () {
    // 전체 화면내용이 다시 렌더링된 후에 아래의 코드가 실행됩니다. 
  })
}
```

### vm.$nextTick

<aside>
💡 다음 DOM 업데이트 주기 이후 실행될 콜백을 연기하며, DOM 업데이트를 기다리기 위해 일부 데이터를 변경한 직후 사용하면 된다.

</aside>

```jsx
createApp() ({
  //...
  methods: {
    example(){
      //data 수정
      this.message = 'changed'
      //DOM은 아직 최신화X
      this.$nextTick(function(){
        //DOM 최신화
        //'this'는 현재 인스턴스에 바인딩됨
      this.doSomethigElse()
      })
    }
  }
})
```

## 4. Destruction ( 해체 단계 )

### beforeDestroy

- 모든 이전 단계를 진행한 후 lifecycle이 해체 되기 직전에 호출된다.

### destroyed

- 해체 된 이후 발생하는 단계
- Vue instance의 모든 디렉티브가 바인딩 해제되고 모든 이벤트 리스너가 제거되면 하위 Vue instance도 삭제된다.

## Instance method / lifeCycle

**vm.$nextTick( [callback] )**

전달인자 {Function} {callback}

<aside>
💡 다음 DOM 업데이트 사이클 이후 실행될 callback을 연기한다. DOM 업데이트를 기다리기 위해 일부 데이터를 변경한 직후 사용하면 되고 콜백의 this 컨텍스트가 이 메소드를 호출하는 instance에 자동으로 바인딩되는 점을 제외하면 전역 Vue.nextTick과 같다.

</aside>

```jsx
new Vue({
//...
	methods: {
		example: function(){
			//데이터 수정
		this.message = 'changed'
		//아직 DOM이 갱신되지 않음
		this.$nextTick(function(){
		//DOM이 이제 갱신됨
		//this가 혀ㅛㄴ재 인스턴스에 바인딩 됨
		this.doSomethingElse()
		})
}
```

[Vue.js - The Progressive JavaScript Framework | Vue.js](https://v3-docs.vuejs-korea.org/)