---

layout: post

title: Main project Day 16

subtitle: TIL

categories: coding

tags: [React, project]

---
**2022-09-25**

어제 잠을 좀 잤더니 개운개운했다. 

어느정도 페이지가 나와서 app.js가 더러워져 router 를 조금 정리할 필요가 생겼고 로그인에 따라 privateRoute 를 지정할 필요가 생겨 작업을 진행했다.

footer 부분은 볼 수록 크기가 큰것 같아 디자인을 조금 수정하였다.

분명히 gitignore에 env파일을 지정했는데 status를 보다가 env가 포함되어있는 것을 보고 식겁했다. 다시확인해도 포함이 되어있는데….

## Routers


```jsx
export const routerList = [
  {
    id: 1,
    path: "/",
    isPrivate: false,
    element: <MainPage />,
  },
  {
    id: 2,
    path: "/login",
    isPrivate: true,
    element: <LoginPage />,
  },
  {
    id: 3,
    path: "/signup",
    isPrivate: true,
    element: <SignUpPage />,
  },
  {
    id: 4,
    path: "/detail/:id",
    isPrivate: false,
    element: <ProductDetailPage />,
  },
  {
    id: 5,
    path: "/product-register",
    isPrivate: true,
    element: <ProductRegisterPage />,
  },
  {
    id: 6,
    path: "/shop/*",
    isPrivate: false,
    element: <ShopPage />,
  },
  {
    id: 7,
    path: "/mypage/*",
    isPrivate: true,
    element: <MyPage />,
  },
  {
    id: 8,
    path: "/cart",
    isPrivate: true,
    element: <CartPage />,
  },
  {
    id: 9,
    path: "*",
    isPrivate: false,
    element: <NotFoundPage />,
  },
];
```

```jsx
<Routes>
            {routerList.map((v) => {
              return (
                <Route
                  key={v.id}
                  path={v.path}
                  element={
                    v.isPrivate ? (
                      <PrivateRoute isLogin={isLogin} component={v.element} />
                    ) : (
                      v.element
                    )
                  }
                />
              );
            })}
          </Routes>
```

app.js의 번잡함을 최소화하고 싶어 각 router를 파일로 분리해 id를 부여하고 app.js에서 사용하였다. private를 지정할 때 어떠한 방식이 좋을까 생각하다 위와 같은 방식을 택했지만 더 좋은 방식의 관리법은 없는지 찾아봐야겠다.

## Todo


슬슬 API가 하나씩 나왔으면 하지만 마냥 재촉할 수 없으니 내가할 수 있는 일부터 처리해야겠다. 같은 FE 파트 팀원분이 아직 어려움을 겪고 있어 내일도 오전부터 같이 문제를 해결하기로했다.

팀장을 맡고나서 내가 잘 하고있는건가 싶지만 할수 있는건 다 하고있으니 더 노력하자.