---

layout: post

title: Main project Day 21

subtitle: TIL

categories: coding

tags: [React, project]

---

**2022-09-30**

상품 API가 나와서 바로 연결하고 테스트를 진행했다. 다행히 어제 등록한 아이템들도 잘 들어와 mainItems, detail, shop 페이지가 좌라락 완성되어서 기분이 너무 좋았다.

이제 정말 쇼핑몰 같은 느낌이 들어 코딩할 맛이 난다.

## shop


![Untitled](/post-img/figma15.png)

데이터를 뿌리고 필터부분까지 완성하니 잘 동작하였다. 지금까지 준비했던게 헛수고가 아닌것 같아 다행이었다. 다만 데이터 호출이 빨라 로딩 페이지가 오히려 사용자 경험에 좋지 않을듯해서 이후 딜레이를 주고 스켈레톤으로 변경해야겠다. 

```jsx
export default function useGetProductItems(params, setFunction) {
  const { data, isLoading, isSuccess, isError, refetch } = useQuery(
    ["getItems", params],
    () => getProductItems(params),
    {
      retry: 2,
      staleTime: 1000 * 60 * 30,
      onSuccess: () => {
        setFunction(true);
        setTimeout(() => {
          setFunction(false);
        }, 1200);
      },
    }
  );
```

미리 스켈레톤을 대비해 잠시 딜레이를 주고 staleTime을 30분으로 지정하였다. 쇼핑몰의 경우 아이템이 업데이트 되는 시점이 엄청 빠를 필요는 없을거라 판단해 만료시간을 조금은 길게 두었다.

## Todo


내일은 Cart API가 완성될 수 있다고 하셔서 오늘 받은 API를 활용한 부분을 최대한 완성했다. 몸은 너무 피곤한데 주요 API가 하나 둘 나오니 정신적으로는 스트레스가 크지 않아 다행이다.

FE 팀원분이 맡아주신 review, QNA 부분의 API가 아직 난항이라 멘토님과 상의 후 로컬에서라도 동작할 수 있도록 준비하기로 했다. 페이지 동작에 문제가 없도록 어떻게든 진행하라고 말씀을 하셨는데 이부분은 전적으로 동의하기 때문에 내가 맡은 부분도 혹시 몰라 준비할 필요가 있을듯하다.