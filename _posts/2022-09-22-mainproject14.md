---

layout: post

title: Main project Day 14

subtitle: TIL

categories: coding

tags: [React, project]

---
**2022-09-22**

로그인 시 Header 메뉴 변경과 redux 테스트를 위해 GoogleOauth를 클라이언트에서 지정해 테스트하였다. redux-persist로 로그인 정보와 상태를 session에 저장하고 header UI를 변경하는 형식으로 진행했는데 다행히 큰 문제 없이 잘 동작하였다.

어제 계획했던데로 팀원분이 맡은 review, qna 쪽과 itemCard 부분을 같이 진행했는데 생각보다 시간이 많이 걸려 itemCard 쪽은 따로 작업해서 설명해드리기로 했다.

개인 포트폴리오가 아니라 팀 포트폴리오로 활용될 프로젝트라 팀원분을 기다리면서 진행해야할지 우선 완성을 목표로 작업해야할지 판단하기가 힘들지만 우선 내 작업을 빠르게 마무리해야 도와드릴 수 있을 것 같아 작업을 진행했다.

## itemCard


![Untitled](/post-img/figma10.png)

아이템 카드부분에 찜하기와 카드전체가 모두 기능을 가지고 있어 이벤트 버블링을 처리했다. 기존에 팀원분이 작업해주신 부분에 반응형이 가능하도록 css를 조정했다.

## Todo


메인페이지 구현 중에 BestSeller 등의 카테고리 필터가 기한내에 힘들다는 백엔드 팀원들의 의견이 있어 그 부분을 채울 디자인과 기능을 생각해야겠다.

GoogleOAuth 부분을 백엔드에서 redirect 하는 방식으로 진행한다고 하여 API가 나오면 변경하여 테스트가 필요하다. API가 나오기전 최대한 Client 부분을 완성시켜야하니 내일도 빡세게 작업해야겠다.