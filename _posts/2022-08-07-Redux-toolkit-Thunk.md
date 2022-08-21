---

layout: post

title: 22 / 08 / 07 My TIL [Redux-toolkit Thunk]

subtitle: SEB TIL

categories: coding

tags: [React]

---
# Redux-toolkit thunk

<aside>
💡 thunk란 특정 작업을 나중에 처리할 수 있도록 함수형태로 감싼 것을 의미한다.

기존의 Redux에서 비동기 처리를 위해서는 `redux-thunk` 미들웨어를 추가해야한다. 하지만 Redux-toolkit 에서는 기본적으로 thunk가 추가되어 있기 때문에 `createAsyncThunk` API를 사용해 비동기 상태를 작성할 수 있다.

</aside>

## thunk 생성

```jsx
createAsyncThunk(typePrefix: string, payloadCreator: AsyncThunkPayloadCreator, options?: AsyncThunkOptions): AsyncThunk
```

- 기본 구성

```jsx
export const getRecentPosts = createAsyncThunk('posts/getRecentPosts', async () => {
  const response = await axios({ baseURL: API_HOST, url: '/recentPosts' });
  return (await response.data.json()) as PartialPost[];
});
```

- 예제

<aside>
💡 위와 같이 `createAsyncThunk` 를 선언하게 되면 인자로 전달한 Action 이름에 `pending` , `fulfilled` , `rejected` 의 상태에 대한 Action을 자동으로 생성한다.

</aside>

## Role

> 인자로 받은 typePrefix를 기반으로 Promise의 라이프사이클 Action Type을 생성한다.

인자로 받은 Promise callback을 실행하고 Promise 라이프 사이클 Action을 dispatch하는 action creator를 반환한다.

createAsyncThunk 는 사용자가 데이터를 어떻게 처리할지 모르기 때문에 reducer 함수를 생성하지 않는다.
> 

## example

```jsx
import { createAsyncThunk, createSlice } from '@reduxjs/toolkit'
import { userAPI } from './userAPI'

const fetchUserById = createAsyncThunk(
  'users/fetchByIdStatus',
  async (userId, thunkAPI) => {
    const response = await userAPI.fetchById(userId)

    return response.data
  }
)

const usersSlice = createSlice({
  name: 'users',
  initialState: { entities: [], loading: 'idle' },
  reducers: {},
  // extraReducers에 케이스 리듀서를 추가하면 
  // 프로미스의 진행 상태에 따라서 리듀서를 실행할 수 있습니다.
  extraReducers: (builder) => {
    builder
      .addCase(fetchUserById.pending, (state) => {})
      .addCase(fetchUserById.fulfilled, (state, action) => {
	      state.entities.push(action.payload)
      })
      .addCase(fetchUserById.rejected, (state) => {})
  },
})

// 위에서 fetchUserById, 즉 thunk를 작성해두고
// 앱에서 필요한 시점에 디스패치 하여 사용합니다.

// ...

dispatch(fetchUserById(123))
```

## Components Example

```jsx
const ReactComponent = () => {
  const { openDialog } = useDialog();
  
  // (아래 GIF처럼 버튼의 onClick 액션을 핸들링하는 함수입니다.)
  const handleSubmit = async (): Promise => {

    // 화면에 띄울 다이얼로그를 선언하고, 프로미스 결과를 기다립니다.
    // 사용자가 '동의' 버튼을 누르면 true로 평가됩니다.
    const hasConfirmed = await openDialog({
      title: '데이터 전송',
      contents: '입력한 데이터를 전송할까요?',
    });

    if (hasConfirmed) {
      // 이후 비즈니스 로직 실행
    }
  };
}
```

```jsx
const useDialog = () => {
  const dispatch = useAppDispatch();

  // 리액트 컴포넌트에서 훅을 사용해서 openDialog 함수를 호출했다면
  // 썽크(thunk) 액션 생성자 함수를 통해서 액션을 디스패치하게 됩니다.
  const openDialog = async (state: DialogContents): Promise => {
    const { payload } = await dispatch(confirmationThunkActions.open(state));

    return payload
  };

  // ...
	
  return {
    openDialog,
    ...
  }
};
```

```jsx
const confirmationThunkActions = {
  open: createAsyncThunk<
    boolean,
    DialogContents,
    { extra: ThunkExtraArguments }
  >('dialog', async (payload, { extra: { store }, dispatch }) => {
    // thunk 액션이 실행되고, 실제로 다이얼로그가 열리는 부분입니다.
    dispatch(openDialog(payload));

    return new Promise<boolean>((resolve) => {

      // 스토어를 구독하고 상태 변경을 감지하면
      // 사용자의 '동의', '거절' 액션에 맞추어 resolve 처리합니다.
      const unsubscribe = store.subscribe(() => {
        const { dialog } = store.getState() as RootState;

        if (dialog.isConfirmed) {
          unsubscribe();
          resolve(true);
        }

        if (dialog.isDeclined) {
          unsubscribe();
          resolve(false);
        }
      });
    });
  }),
};

export default confirmationThunkActions;
```

---
<br><br>
## 마무리

