---

layout: post

title: Main project Day 10

subtitle: TIL

categories: coding

tags: [React, project]

---
**2022-09-18**

어제 react-query를 살펴보고 기본셋팅과 errorHandler를 전역으로 지정해 셋팅해 두었다. 에러 발생 시 axios에러와 status에 따라 동작을 다르게 구분하고 refetch 타이밍을 조정해 두었는데 아직 익숙하지 않아 개발을 진행하며 조금씩 수정이 필요할 것 같다.

## Query Client


```jsx
export const errorHandler = async (error) => {
  if (error instanceof AxiosError) {
    if (!error.response?.data) {
      toast.error(error.message);
      return;
    } else if (error.response?.data.error) {
      toast.error(error.response?.data.error);
      return;
    } else if (error.response?.data.message) {
      toast.error(error.response?.data.message);
      return;
    }
  }

  toast.error("error!");
  return;
};

export const queryClient = new QueryClient({
  defaultOptions: {
    queries: {
      refetchOnWindowFocus: false,
    },
  },
  queryCache: new QueryCache({
    onError: errorHandler,
  }),
  mutationCache: new MutationCache({
    onError: errorHandler,
  }),
});
```

에러가 발생할 때 마다 콘솔을 확인하는게 번거로워 일단 `react-toastify` 를 활용해 에러 발생을 확인하기로 했다. 추후 에러 모달을 커스텀으로 제작하는 것이 좋을것 같다.

react-query를 실습하다 콘솔에 찍어둔 데이터가 계속 호출되어 이상한것 같아 찾아보니 `refetchOnWindowFocus` 이 default로 지정되어있어 창을 나갔다 올 때 마다 호출하는 것을 알게되어 기본 값을 false로 지정해 두었다.

## Todo


내일은 예정대로 실제 개발에 들어가야한다. 오전에 디자인과 로고 마무리를 하고 API가 나오기 전 레이아웃 작업을 시작해 일정에 차질이 없도록 해야한다.

뭔가 다른팀은 진전이 어떻게 되어가고 있는지 몰라서 우리팀의 진행 속도가 빠른지 아닌지 불안한 구석도 있지만 신경쓸 필요는 없을것 같다.

스터디 원들이 속해있는 팀의 반 정도가 디자이너를 따로 영입했다고 하는데 같은 과정에 있지않을 경우 책임감이나 시간을 엄수와 같은 변수가 있어 내가 진행하고 있지만 조금은 벅찬 느낌도 들긴하다 ㅜㅠ