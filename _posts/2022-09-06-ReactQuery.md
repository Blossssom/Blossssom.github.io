---

layout: post

title: 22 / 09 / 06 My TIL [React Query]

subtitle: TIL

categories: coding

tags: [React, react-query]

---
# React-query


<aside>
💡 Server 상태를 관리하는 react-query에 대해 알아보자.

</aside>

## Client State Vs Server State

---

react-query 를 알아보기에 앞서 서버상태가 무엇인지에 대해 먼저 알아볼 필요가 있다.

### Client State

- Client 상태는 웹 브라우저 세션과 관련된 모든 정보를 의미한다.
- 테마 변경, UI 변경 등 서버에서 일어나는 일과 관련이 없는 상태를 뜻한다.

 

### Server State

- Server 상태는 서버에 저장되지만 클라이언트를 나타내는데 필요한 데이터를 의미한다.
- DB에 저장될 데이터, 이미 저장되어있는 데이터 처럼 여러 클라이언트에 표시할 수 있도록 저장된 데이터를 뜻한다.

각각의 상태가 의미하는 바가 다르단건 알겠는데 왜 굳이굳이 react-query를 사용하는 걸까?

## Why?

---

react-query는 클라이언트에서 서버데이터 캐시의 관리를 가능케하는 라이브러리이다. 이를 통해 React Client에서 서버 데이터가 필요할 때 흔히 사용하는 `fetch` 나 `axios` 를 바로 사용하지 않고 react-query 캐시를 요청하게 된다.

하지만 캐시가 항상 최신상태를 보장할 수 있지 않기에 새로운 데이터로 캐시를 업데이트 하는 시기는 사용자의 설정에 따라 지정해야한다.

```jsx
key: 'post-data',
data: [
	1: {
		title: 'react-query',
		tagLine: 'is good'
	},

	2: {
		title: 'menage state',
		tagLing: 'is hard'
	}
]
```

위 코드는 react-query 캐시의 예시이다. 각각의 캐시는 `key` 에 해당하는 value로 관리된다.

캐시가 서버의 데이터와 같이 최신 상태로 유지되기 위해 서버의 데이터와 일치하는지 확인해야하는데 크게 두가지 방법이 있다.

- 명령형 처리 : query 클라이언트의 데이터를 무효화하고 교체할 캐시 데이터를 서버에서 불러온다.
- 선언형 처리 : refetch를 트리거하는 조건을 구성해 데이터를 구성한다. (ex - focus, staleTime 등의 기준)

## Other

---

서버 상태를 관리하는 기능 외에도 react-query는 서버 상태 관리를 위한 여러 도구를 제공한다.

### Error state / Loading

- react-query는 서버에 대한 모든 로딩 및 오류 상태를 유지, 관리하기 때문에 수동으로 관리할 필요가 없다.

### Pagenation / Infinite scroll

- 사용자를 위한 pagenation, 무한 스크롤을 제공한다.

### Prefetch

- 사용자가 데이터를 필요로하는 시점을 파악해 미리 데이터를 가져와 캐싱할 수 있다. 미리 캐싱된 데이터는  서버에서 다시 가져올 필요가 없으므로 불필요한 서버의 호출을 줄일 수 있다.

### Mutations

- 단어의 뜻대로 서버 데이터의 변이와 업데이트를 관리할 수 있다.

### De-duplication of requests

- 각각의 쿼리가 key로 식별되기 때문에 요청을 관리할 수 있고 여러 요소에서 같은 데이터를 요청할 경우 한번에 요청으로 처리할 수 있다.

### Retry

- 서버에서 오류가 발생하는 경우 재시도를 관리할 수 있다.

### Callbacks

- 쿼리 요청의 성공, 오류를 구별해 조치를 취할 수 있도록 콜백을 전달할 수 있다.

---
<br><br>
## 마무리
pre project를 진행하면서 TIL을 너무 소홀히 하고 있었다...
서버 데이터의 상태 관리에 대해 고민하다 React Query를 알게되었는데 아직 기존의 방식과 차이를 느낄 수 없어 많은 레퍼런스와 학습이 필요하다.

