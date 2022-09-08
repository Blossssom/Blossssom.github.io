---

layout: post

title: 22 / 09 / 07 My TIL [React Query basic]

subtitle: TIL

categories: coding

tags: [React, react-query]

---
# React-query basic

<aside>
💡 react-query의 기본 사용법에 대해 알아보자.

</aside>

## QueryClient

---

`QueryClient` 는 react-query의 캐시 데이터를 관리하는 역할로 사용 시 인스턴스를 생성하여 해당 QueryClient로 접근할 수 있도록 지정이 필요하다.

```jsx
import { QueryClient } from "react-query";

const queryClient = new QueryClient();
```

## QueryClientProvider

---

위에서 생성한 QueryClient에 접근할 수 있도록 컴포넌트 트리 상위에 `QueryClientProvider` 를 지정해 하위 컴포넌트에서도 캐시 데이터를 공급받을 수 있도록 지정해야한다.

```jsx
import { Posts } from "./Posts";
import "./App.css";
import { QueryClient, QueryClientProvider } from "react-query";

const queryClient = new QueryClient();

function App() {
  return (
    // QueryClient의 캐시 데이터 공급을 위한 지정
    <QueryClientProvider client={queryClient}>
      <div className="App">
        <h1>Blog Posts</h1>
        <Posts />
      </div>
    </QueryClientProvider>
  );
}

export default App;
```

---
<br><br>


