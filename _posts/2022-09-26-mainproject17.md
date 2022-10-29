---

layout: post

title: Main project Day 17

subtitle: TIL

categories: coding

tags: [React, project]

---

**2022-09-26**

로그인 API가 먼저 나와서 테스트를 진행했는데 AccessToken 관리를 어떻게 할지 고민이다. 게다가 RefreshToken은 시간이 더 필요하다고 하셔서 refresh를 염두해 두고 진행해야겠다.

온갖 글을 찾아보고 가장 안전한 방식을 택하려했는데 백엔드 파트 분들과 이야기를 했는데 서로 이해하는 부분이 달라 애먹었지만 잘 해결되었다.

- cors에러 문제가 생겨 백엔드 파트분들과 회의하며 다같이 문제를 해결했다. 전에 잠깐이나마 JAVA 백엔드를 경험해봐서 다행이었다…

## Routers


```jsx
import axios from "axios";

export const axiosInstance = axios.create({
  headers: { "Content-Type": "application/json" },
  withCredentials: true,
});

axiosInstance.defaults.timeout = 5000;

axiosInstance.interceptors.response.use(
  (config) => {
    return config;
  },
  (err) => {
    return Promise.reject(err);
  }
);
```

쿠키로 토큰을 받기위해 withCredentials 를 지정해 axiosInstance로 사용했다. timeout의 경우 5초가 짧을지 길지 판단이 서지 않아 멘토님께 여쭤봤는데 적당하다는 피드백을 받아 이대로 진행해야겠다.

## Todo


코스 과정이 끝을 달리고 메인 프로젝트도 절반쯤 오니 팀원들이 지쳐가는게 보인다. 가끔 팀원들이 새벽에 상담요청을 하는데 개인적인 생각이나 고민을 털어놔 주셔서 신뢰를 받고 있는 느낌이지만 같은 수강생 입장으로서 어떠한 이야기를 꺼내기가 조심스러워 잘 진행하고 있다고 말해드리는 것 외엔 할 수가 없어 답답할 때가 있다.

내일은 로그인 에러처리, 회원가입, privateRouter 테스트를 진행해야한다. 같은 FE 파트분과 BE 팀원 분이 진행에 어려움을 겪고 있는데 최대한 같이 가야한다는 생각과 팀 포트폴리오를 어떻게든 완성해야한다는 입장이 겹쳐 밸런스 조정이 필요하다.