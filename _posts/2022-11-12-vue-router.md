---

layout: post

title: Vue - router

subtitle: TIL

categories: coding

tags: [Vue, Study]

---

# Vue Router

<aside>
💡 spa 에서 페이지 간 이동을 위한 router에 대해 알아보자. spa는 DOM이 새로 갱신되는 것이 아닌 부분업데이트와 리렌더링이 이뤄지는 만큼 어떻게 동작하는지도 정확하게 알아야한다.

vue-router는 npm 혹은 yarn으로 설치가 가능하지만 vue/cli 로 프로젝트를 생성할 때 지정도 가능하다.

router는 페이지의 사용성, 최적화와 연관이 되어있는 만큼 설계단계에서 심도있게 설계가 필요하다.

</aside>

## router.js

```jsx
import { createWebHistory, createRouter } from "vue-router";

const routes = [
  {
    path: "ex) /list",
    component: ex) List,
  },
	{
    path: '/about',
    name: 'about',
    component: () =>
      import(/* webpackChunkName: "about" */ '../views/AboutView.vue')
  }
];

const router = createRouter({
  history: createWebHistory(),
  routes,
});

export default router; 
```

- path : 경로, `router-link`의 to 속성에 지정된 경로와 동일해야한다.
- name : 유니크한 이름으로 설정하되 컴포넌트의 기능을 알 수 있도록 지정해야한다.
- component : 해당 router로 가져올 컴포넌트를 지정한다.

## App.vue

```jsx
<template>
  <nav>
    <router-link to="/">Home</router-link> |
    <router-link to="/about">About</router-link>
  </nav>
  <router-view />
</template>
```

- vue-router의 기본적인 구성은 다음과 같이 되어있다. routes 부분은 createRouter에 작성할 수 있지만 위 코드 레퍼런스와 같이 나눠 작성하는 것이 훨씬 깔끔해보인다.
- path와 name 기준으로 라우팅 될 컴포넌트 지정이 조금씩 달라보이는데 함수로 지정되어 있는 라우터의 경우 lazy loading을 지원한다.
- 특이한 점은 lazy loading 부분의 import 내부 주석 처리 부분이 실제 코드로서 동작한다는 점이다. 이러한 Dynamic import는 prefetch 지정에 따라 의도와 다르게 흘러갈 수 있으니 추후에 더 알아보자.

<aside>
💡 Dynamic import에 지정한 `webpackChunkName` 는 웹팩에서 애플리케이션 코드를 각각 나눈 것을 나타내며 위와 같이 따로 지정할 경우 해당 컴포넌트의 output 파일명을 변경할 수 있다.

만약 `webpackChunkName` 이 동일할 경우 하나의 파일로 합쳐진다. 즉, 서브메뉴와 같이 가볍고 사용빈도가 높은 구성들을 그룹핑하여 webpack 에서 바로 내려받을 수 있는 구조가 되므로 컴포넌트 혹은 페이지의 구성에 따라 모듈을 그룹핑 하여 사용하면 효과적이다.

</aside>

## Lazy Loading

<aside>
💡 서비스를 구현하다보면 페이지에 따라 중요도에 따라 기능을 나눌 수 있다. 따라서 중요한 요소를 먼저 다운로드하여 구성하고 후순위의 컴포넌트 혹은 기능을 나중에 다운로드하여 페이지 성능 향상을 끌어올릴 수 있다.

</aside>

```jsx
const routes = [
  {
    path: "ex) /list",
    component: ex) List,
  },
	{
    path: '/about',
    name: 'about',
    component: () =>
      import(/* webpackChunkName: "about" */ '../views/AboutView.vue')
  }
];
```

- `import(...)` 을 활용하여 Dynamic import를 진행할 모듈을 지정하면 웹팩이 해당 코드를 만났을 때 별도의 chunk로 분리해 다운로드를 진행한다.

## webpackPrefetch

<aside>
💡 react-query에서 페이지 네이션을 지정할 때 사용했던 prefetch 처럼 미리 컴포넌트를 가져와 캐시에 담아두는 것 또한 가능하다. 

미리 불러놓은 모듈을 불러오는 만큼 해당 모듈의 로딩은 짧아지지만 첫 렌더링 과정에서 동작이 이뤄지기 때문에 첫 렌더링의 시간이 더 소요될 수 있다.

개발자도구의 element - head 부분을 확인해 보면 prefetch한 모듈이 명시되어있다.

</aside>

```jsx
const routes = [
  {
    path: "ex) /list",
    component: ex) List,
  },
	{
    path: '/about',
    name: 'about',
    component: () =>
      import(/* webpackChunkName: "about", webpackPrefetch: true */ '../views/AboutView.vue')
  }
];
```

- 위와 같이 Dynamic import 내부에 지정하여 사용할 수 있다.

<aside>
💡 webpackPrefetch와 lazy loading 모두 각각의 장단점이 있는 만큼 필요에 따라 사용해야한다. 특히 페이지의 유저 플로우와 해당 페이지, 컴포넌트의 사용률에 따라 보다 유리하게 사용가능하니 초기단계에서 잘 설계해보자.

</aside>