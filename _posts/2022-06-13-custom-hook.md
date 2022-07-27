---

layout: post

title: Custom Hook

subtitle: SEB TIL

categories: coding

tags: [javascript, React]

---

## Custom Hook

<aside>
`useState, useEffect` 와 같이 사용하기 편리한 hook을 선언하여 custom hook을 사용해보자

</aside>

```jsx
import { useEffect, useState } from 'react';
import {useParams} from 'react-router-dom'
import Word from './Word';

function Day() {
    const getDay = useParams();
    const day = getDay.day;
    const [words, setWords] = useState([]);

    useEffect(() => {
        fetch(`http://localhost:3001/words?day=${day}`)
        .then((res) => {
            return(res.json());
        })
        .then((wordList) => {
            setWords(wordList);
        })
    }, [day]);

    return(
        <>
            <h2>Day {day}</h2>
            <table>
                <tbody>
                    {words.map((word) => (
                        <Word word={word} key={word.id} />
                    ))}
                </tbody>
            </table>
        </>
    );
}

export default Day;
```

<aside>
기존에 작성한 Day 컴포넌트의 fetch 부분을 다른 컴포넌트에서도 간단하게 사용하기 위해 custom hook으로 따로 빼내자

</aside>

```jsx
import { useState, useEffect } from 'react';

function useFetch(url) {
    const [data, setData] = useState([]);

    useEffect(() => {
        fetch(url)
        .then((res) => {
            return(res.json());
        })
        .then((wordList) => {
            setData(wordList);
        })
    }, [url]);

    return data;
}

export default useFetch;
```

<aside>
일반적인 hook 처럼 useFetch로 이름을 선언하고 상황마다 변경될 수 있는 url부분을 파라미터로 받는 hook을 선언하였다. 결국 `useFetch` 로 얻을 것은 받아온 데이터기 때문에 마지막으로 data를 반환했다.

</aside>

```jsx
import { useEffect, useState } from 'react';
import {useParams} from 'react-router-dom'
import useFetch from '../hooks/useFetch';
import Word from './Word';

function Day() {
    const getDay = useParams();
    const day = getDay.day;
    const dbUrl = `http://localhost:3001/words?day=${day}`;

    const words = useFetch(dbUrl);

    return(
        <>
            <h2>Day {day}</h2>
            <table>
                <tbody>
                    {words.map((word) => (
                        <Word word={word} key={word.id} />
                    ))}
                </tbody>
            </table>
        </>
    );
}

export default Day;
```

<aside>
이후 기존 컴포넌트에서 사용했던 fetch 부분을 `useFetch` 로 변경하여 작성하였다.

</aside>

<aside>
커스텀 훅을 각각 찍어보며 사용하다가 렌더링이 두번씩 되는 상황을 발견했다. 
구글링 결과 **Strict Mode** 때문인데 개발 모드에서 side effect를 발견하기 쉽게 적용해놓은 기능이라 배포단계에선 정상적으로 렌더링이 이루어진다. ****

</aside>

---
<br><br><br>
## 마무리

여러 훅을 조합해 필요한 기능을 Custom Hook으로 만든다는 자체가 새로웠던 것 같다. 제공하는 메서드에 대해 단순히 사용만 하는 것이 아니라 상황에 맞게 커스텀하는 자세가 필요할 것 같다.

