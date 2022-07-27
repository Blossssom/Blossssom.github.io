---

layout: post

title: useEffect

subtitle: SEB TIL

categories: coding

tags: [javascript, React]

---

[Using the Effect Hook - React](https://ko.reactjs.org/docs/hooks-effect.html)

## useEffect?

effect hook은 컴포넌트가 렌더링 될 때 내부 작업을 실행할 수 있도록 하는 Hook 이다.

effect를 사용하면 함수형 컴포넌트에서도 side effect를 사용할 수 있는데, 기존 Class형 컴포넌트에서 사용할 수 있던 생명주기 Method (componentDidMount, componentDidUpdate 등)를 사용할 수 있게된다.

## 선언방법

```jsx
import react, { useEffect } from 'react;

useEffect(() => {
	// effect
}, []);
```

첫 번째 인자(effect)는 콜백함수로, 두 번째 인자는 배열이 들어간다. 특히 뒤에 들어가는 배열은 의존성 배열로 effect에서 중요한 역할을 하니 진행하며 더 깊게 다뤄보자.

## effect

React는 렌더링 이후 실행할 effect 함수를 기억해 DOM 업데이트를 완료한 후 불러낸다.

만약 effect 함수에서 다른 함수를 return할 경우 해당 함수는 컴포넌트가 Unmount 될 때 정리의 개념으로 한 번 실행된다.

```jsx
import { Link } from 'react-router-dom';
import { useState, useEffect } from 'react';

function DayList() {
    const [days, setDays] = useState([]);
    const [count, setCount] = useState(0);

    function onClick() {
        setCount(count + 1);
    }

    useEffect(() => {
        console.log('cnt change');
    });

    return(
        <ul className='list_day'>
            {days.map((day) => (
                <li key={day.id}>
                    <Link to={`/day/${day.day}`}>
                        Day {day.day}
                    </Link>
                </li>
            ))}
            <button onClick={onClick}>{count}</button>
        </ul>
    );
}

export default DayList;
```

위 코드를 실행하면 첫 렌더링이 되었을 때, count state가 변경되며 렌더링 될 때 마다. 내부 console이 실행된다. 이는 componentDidMount와 componentDidUpdate 를 함께 표현한 것으로 볼 수 있다.

위에서 설명한 의존성 배열이 아직 선언되지 않았다는 것을 숙지하고 코드를 살펴보자.

## 의존성 배열

이번엔 useEffect 의 인자인 의존성 배열을 선언하고 어떠한 차이가 있는지 살펴보자.

```jsx
import { Link } from 'react-router-dom';
import { useState, useEffect } from 'react';

function DayList() {
    const [days, setDays] = useState([]);
    const [count, setCount] = useState(0);

    function onClick() {
        setCount(count + 1);
    }

    useEffect(() => {
        console.log('cnt change');
    }, []);

    return(
        <ul className='list_day'>
            {days.map((day) => (
                <li key={day.id}>
                    <Link to={`/day/${day.day}`}>
                        Day {day.day}
                    </Link>
                </li>
            ))}
            <button onClick={onClick}>{count}</button>
        </ul>
    );
}

export default DayList;
```

위 처럼 빈 배열을 입력하고 실행하면 componentDidMount와 같이 첫 렌더링 때만 effect가 실행되는 것을 볼 수 있다. 이처럼 빈 의존성 배열을 선언하면 컴포넌트의 Mount 시에만 실행되며 특정 값이 변경될 때 effect를 실행시키고 싶다면 배열 내에 그 값을 넣어줘야한다.

```jsx
import { Link } from 'react-router-dom';
import { useState, useEffect } from 'react';

function DayList() {
    const [days, setDays] = useState([]);
    const [count, setCount] = useState(0);
		const [name, setName] = useState('');

    function onClick() {
        setCount(count + 1);
    }

		function inputChange(e) {
				setName(e.target.value);	
		}

    useEffect(() => {
        console.log('cnt change');
    }, [count]);

    return(
        <ul className='list_day'>
						<input />
            {days.map((day) => (
                <li key={day.id}>
                    <Link to={`/day/${day.day}`}>
                        Day {day.day}
                    </Link>
                </li>
            ))}
            <button onClick={onClick}>{count}</button>
        </ul>
    );
}

export default DayList;
```

이번엔 의존성 배열에 count state를 할당하였다. 이렇게 값을 지정할 경우 첫 마운트 때와 지정한 count 가 변경될 때만 동작한다.

### Cleanup 함수

cleanup 함수는 useEffect 내부에서 return될 때 실행되는 뒷정리를 위한 함수이다.

만약 component가 마운트 될 때 이벤트 리스너를 사용해 이벤트를 추가할 경우 반드시 이벤트를 삭제해 주어야 한다. 그렇지 않으면 리렌더링 될 때마다 새로운 이벤트 리스너가 핸들러에 바인딩 되어 메모리 누수가 발생하기 때문이다.

```jsx
import { Link } from 'react-router-dom';
import { useState, useEffect } from 'react';

function DayList() {
    const [days, setDays] = useState([]);
    const [count, setCount] = useState(0);
		const [name, setName] = useState('');

    function onClick() {
        setCount(count + 1);
    }

		function inputChange(e) {
				setName(e.target.value);	
		}

    useEffect(() => {
        console.log('cnt change');
				
				return () => {
						console.log('clean up!');
				}
    }, [count]);

    return(
        <ul className='list_day'>
						<input />
            {days.map((day) => (
                <li key={day.id}>
                    <Link to={`/day/${day.day}`}>
                        Day {day.day}
                    </Link>
                </li>
            ))}
            <button onClick={onClick}>{count}</button>
        </ul>
    );
}

export default DayList;
```

위 코드를 실행해보면 버튼 클릭 시 먼저 cleanup 함수가 실행되어 이전 count를 출력하고 이후 변경된 count로 effect가 출력되는 것을 볼 수 있다.

따라서 useEffect 함수가 다시 실행될 때 (실행 직전), return 함수를 먼저 실행해 주고 넘어가는 것을 알 수 있다.

## useEffect 활용

실제 useEffect를 사용하는 사례는 대부분 API의 호출일 것이다. 페이지가 렌더링 될 때 데이터를 불러와 뿌려주기 위해 이 과정이 필수적으로 먼저 실행되야하기 때문에 반드시 알아두자.

```jsx
import { Link } from 'react-router-dom';
import { useState, useEffect } from 'react';

function DayList() {
    const [days, setDays] = useState([]);

    useEffect(() => {
        fetch('http://localhost:3001/days')
        .then((res) => {
            return(res.json());
        })
        .then((data) => {
            setDays(data);
        });
    }, []);

    return(
        <ul className='list_day'>
            {days.map((day) => (
                <li key={day.id}>
                    <Link to={`/day/${day.day}`}>
                        Day {day.day}
                    </Link>
                </li>
            ))}
        </ul>
    );
}

export default DayList;
```

서버에 띄운 json 데이터를 호출하기 위해 기존 dommy를 지우고 `fetch` 메서드를 사용해 현재 띄워져 있는 서버에서 json을 불러왔다. 

```jsx
import { Link } from 'react-router-dom';
import { useState, useEffect } from 'react';

function DayList() {
    const [days, setDays] = useState([]);

    useEffect(async() => {
        const res = await(await fetch('http://localhost:3001/days')).json();
        setDays(res);
    }, []);

    return(
        <ul className='list_day'>
            {days.map((day) => (
                <li key={day.id}>
                    <Link to={`/day/${day.day}`}>
                        Day {day.day}
                    </Link>
                </li>
            ))}
        </ul>
    );
}

export default DayList;
```

async 를 사용해 이렇게 구현할 수도 있다. 다만 예외 처리를 추가해야 할듯 하다.

---
<br><br><br>
## 마무리

useEffect는 API 호출, 렌더링 시 동작 등 실제로 활용할 수 있는 사례가 매우 많고 중요하기 때문에 많은 연습과 학습이 필요한 부분같다. 

다른 Hook도 사용하면서 이후 한번 더 깊게 알아봐야겠다.

