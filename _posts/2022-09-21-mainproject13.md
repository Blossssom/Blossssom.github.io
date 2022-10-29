---

layout: post

title: Main project Day 13

subtitle: TIL

categories: coding

tags: [React, project]

---
**2022-09-21**

Cart 페이지 작업이전에 기존에 작업하던 productRegister와 detail 쪽 selector 부분에서 난감한 상황을 겪어 시간을 좀 날렸다…

특히 colorSelector 부분에 체크박스를 활용하려 했는데 체크박스에 커스텀 스타일로 배경색을 지정할 수 없다는 걸 알게되어 하나하나 만들 수 밖에 없었다.

## Color selector


![Untitled](/post-img/figma08.png)

역시 figma로 볼 때랑 약간의 차이가 있어 그림자를 추가시켰다. 아예 텍스트를 활용한 태그형식으로 변형할까 했지만 혹시 모르니 복잡한 방식으로 미리 제작을 했다. 단일 선택과 해제가 동작하는 것까지 확인 했으니 register와 detail에서 사용하면 될 것 같다.

## Size selector


![Untitled](/post-img/figma09.png)

사이즈를 선택하는 부분을 슬라이드로 구현하고 싶어 직접 만들었다가 드래그 슬라이드와 여러 문제에 부딪혀 일단 라이브러리로 구현했다.

원래는 대부분 캐러셀에 사용하는 라이브러리지만 동작을 잘하니 다행이다.

## Todo


작업하면서 팀원의 작업물을 병합하고 확인해보니 룰이 어긋난게 많아 내일은 같이 고쳐나가야겠다. 레이아웃이나 CSS에 어려움을 겪으시는 것 같아 조금 작업이 더뎌지고 있어서 내 작업을 빠르게 마무리하고 같이 시도해볼 필요가 있다.