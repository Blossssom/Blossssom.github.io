---

layout: post

title: Main project Day 15

subtitle: TIL

categories: coding

tags: [React, project]

---
**2022-09-23**

shopPage에 들어갈 category, price, color 등의 필터 부분과 page 구현을 진행했다.

브랜드의 경우 계속 추가할 수 있도록 진행하려다 테이블 설계부분이 애매해져 일단 고정된 브랜드로 진행하자는 의견으로 추려졌다.

기존 메인에서 사용하던 배치와 달라져야하고 반응형 또한 배치가 변경되야해서 css 부분에서 수정이 많이 일어났다.

## shopFilter


![Untitled](/post-img/figma12.png)

colorselect를 미리 만들어두어서 다른 부분은 수월하게 진행할 수 있었다. price 필터의 경우 슬라이드로 구현을 진행하려다 막상 적용해보니 눈에 띄지 않고 불편해 직접 입력할 수 있는 형식으로 변경하였다.

## shopItems


![Untitled](/post-img/figma11.png)

shopItems의 경우 main과 달리 3개 씩 보여지게 했다.

이번 프로젝트에선 flex 대신 grid를 사용했는데 이런 카드 배열에 최적화 되어있어 편하게 사용하고 있다.

또한 mainItems 컴포넌트를 재사용하기 위해 props로 어떤 페이지인지 파악해 grid 배열을 변경할 수 있도록 지정하였다.

## Todo


내일은 남겨두었던 배너 이미지 작업과 반응형 작업을 수정하고 학습을 진행해야겠다. 주말에 시간을 사용해야 API가 넘어오기전에 준비할 수 있을것 같아 미리 해둘 필요가 있다. 

잠을 많이 못자서 그런지 지쳐가는데 내일은 잠을 좀 늘려야겠다.