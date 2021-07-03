# Recoil 사용하기 (1)

<br />

## memo

**Selector의 get 함수**

- get 함수는 인자를 가지고 있으며 그 인자값으로는 상태값을 조작할때 사용하는 객체이다.

- 구조 분해 할당을 통해서 인자를 받는 패턴을 사용하는 경우가 많다

  ```react
  export const exampleState = selector({
    key: 'exampleState',
    get: ({get}) => { // destructuring 문법으로 통해서 간결하게 사용할 수 있다.
      return ....
    }
  })
  ```

**Recoil에서는 Hooks 패턴을 사용하여 기존의 Hooks로 React 개발을 해본 사람에게는 이질감 없이 사용할 수 있으며 Atom과 Selector의 구분 없이 동일한 Hook을 사용한다.**

```js
const atomState = atom({
  key: 'atomState',
  default: '' // default 값에는 JS의 모든 타입의 값이 올 수 있다.
});

const selectorState = selector({
  key: 'selectorState',
  get: ({get}) => {
    return ...
  }
});

function App() {
  // 대표적인 Recoil hooks는 useRecoilState, useRecoilValue, useSetRecoilState 등이 존재한다.
  // 및에 보이는 예제에서는 atom이나 selector 둘 다 같은 방식의 hooks를 사용한다.
  const [atom, setAtom] = useRecoilState(atomState);
  const [selector, setSelector] = useRecoilValue(selectorState);

  return (
    <div>{atom}</div>
    <div>{selector}</div>
  );
}
```

<br />

## RecoilRoot

recoil을 사용하기 위해서는 다른 상태 관리 라이브러리(redux, mobx)처럼 초기 설정을 해야한다. 다른 라이브러리와 차이점으로는 휠씬 간결한 초기 설정으로 쉽게 사용할 수 있다는 장점이 존재한다.

redux 라이브로리를 초기 설정하기 위해서는 store를 세팅하고 컴포넌트의 최상단에 provider를 선언 후 provider와 store를 연결해야 하듯이 recoil 또한 최상단의 컴포넌트(상태 관리 라이브러리는 사용할 최상단 루트, 루트 컴포넌트)에 RecoilRoot를 넣어준다.

```js
import React from 'react';
import { RecoilRoot } from 'recoil';

function App() {
  return (
    <RecoilRoot>
      <ExampleComponent />
    </RecoilRoot>
  );
}
```

<br />

## Atom

Atom은 상태의 단위이며, React의 state(상태)와 동일한 의미로 사용되고 있다. 동일한 atom이 여러 컴포넌트에서 사용되는 경우 모든 컴포넌트는 상태를 공유한다. 즉 Atom을 공유하고 있는 컴포넌트는 Atom이 변경되면 리랜더링이 된다.(atom의 값을 읽는 컴포넌트들은 암묵적으로 atom을 **구독**한다.) Atom을 생성하기 위해서는 Recoil의 atom 함수를 사용하며 함수의 인자로는 객체를 넘겨 준다.(필수로 key, default를 작성해야 한다.)

- key: atom 상태를 식별하는 고유한 상태로 두개의 atom이 동일한 key를 갖는 것은 오류를 발생시키는 원인이 되기 때문에 key 문자열은 전역적으로 고유해야 한다.
- default: React state처럼 기본 값을 설정하는 부분으로 JS의 모든 타입이 올 수 있다.

```js
import React from 'react';
import { atom, useRecoilState } from 'recoil';

const countState = atom({
  key: 'countState',
  default: 0,
});

const inputState = atom({
  key: 'inputState',
  default: '',
});

function App() {
  const [count, setCount] = useRecoilState(countState);

  return (
    <div>
      <h1>{count}</h1>
      <button onClick={() => setCount(count + 1)}>increase</button>
      <button onClick={() => setCount(count - 1)}>decrease</button>
    </div>
  );
}

// 간단한 App을 제작할때는 컴포넌트에 atom을 선언해도 되지만 규모가 큰 App을 제작할때는 공용으로 사용하는
// 상태를 파일로 분리하는 것을 추천한다.
```

<br />

## Selector

Atom에서 파생된 데이터 조각 (get 함수를 사용하여 외부 Atom 상태를 받아 올 수 있으며 받아온 Atom을 원하는 데이터 조각으로 조작이 가능), 데이터를 반환하는 순수함수로 정의할 수 있다. 공식 문서에서는 Selector는 atom이나 다른 selector를 이렵으로 받아들이는 순수함수라고 정의하며, 상위의 atom 또는 selector가 업데이트되면 하위의 selector 함수도 다시 실행된다고 설명하고 있다.

Recoil의 전역 상태 관리는 최소한의 상태 집합만을 atom에 저장하고 다른 모든 파생되는 데이터는 selector에 명시한 함수를 통해 효율적으로 계산하여 쓸모없는 상태의 보존을 방지할 수 있다.

selector 함수는 atom 함수처럼 key 값을 가지고 있으며 get(필수) 함수와 set(선택) 함수를 가지고 있다. (따로 defult 값을 지정하는 곳이 존재하지 않는다.)

- `key` : atom 함수와 동일한 고유한 key 문자열(only string type)
- `get` : 파생된 상태의 값을 평가하는 함수(selector의 값으로 생각하자), 값을 직접 반환하거나 비동기적인 Promise 또는 같은 유형을 나타내는 atom이나 selector를 반환한다. 읽기만 가능한 `RecoilValueReadOnly` 객체를 반환한다.
  - 첫 번째 매개변수로는 다음 속성을 포함하는 객체를 전달한다.
  - `get` : 다른 atom이나 selector로부터 값을 찾는데 사용하는 함수, 함수에 전달된 모든 atom과 selector는 암시적으로 이 함수에 대한 의존성 목록에 추가된다.
- `set?` : set 속성을 설정하면 쓰기 가능한 `RecoilState` 객체를 반환한다.
  - `get` : 다른 atom이나 selector로부터 값을 찾는데 사용되는 함수. 이 함수는 selector를 주어진 atom이나 selector를 구독하지 않는다.
  - `set` : 업스트림(비동기) Recoil 상태의 값을 설정할 때 사용되는 함수. 첫 번째 매개변수는 Recoil 상태, 두 번째 매개변수는 새로운 값이다.

**정적 의존성 Selector**

```js
const selectorState = selector({
  key: 'SelectorState',
  get: ({ get }) => get(atomState) + 5,
});
```

**동적 의존성 Selector**

```js
const toggleAtomState = atom({
  key: 'toggleAtomState',
  default: false,
});

const dynamicSelectorState = selector({
  key: 'dynamicSelectorState',
  get: ({ get }) => {
    const toggle = get(toggleAtomState);

    if (toggle) {
      return get(selectorA);
    } else {
      return get(selectorB);
    }
  },
});

// dynamicSelectorState는 toggleAtomState atom 뿐만 아니라 조건에 따라서 selectorA, selectoB Selector에 의존할 수 있다.
```

**비동기 Selector**

```js
import { selector, useRecoilValue } from 'recoil';

const myQuery = selector({
  key: 'MyDBQuery',
  get: async () => {
    const response = await fetch(
      'https://hn.algolia.com/api/v1/search?query=recoil'
    );
    return response.json();
  },
});

function QueryResults() {
  const queryResults = useRecoilValue(myQuery);

  return <div>{queryResults.data}</div>;
}

function ResultsSection() {
  return (
    <React.Suspense fallback={<div>Loading...</div>}>
      <QueryResults />
    </React.Suspense>
  );
}

// 비동기 Selecotr를 사용할 때 주의 사항으로는 React Suspense 컴포넌트를 사용하여 비동기 작업이 끝나기 전까지 보여줄
// component를 미리 작성해야 한다. 그렇지 않을 경우에는 예상치 못한 Error가 발생할 수 있다.
```

**writable Selector**

```js
import { atom, selector, useRecoilState } from 'recoil';

const userState = atom({
  key: 'user',
  default: {
    firstName: 'Gildong',
    lastName: 'Hong',
    age: 30,
  },
});

const userNameSelector = selector({
  key: 'userName',
  get: ({ get }) => {
    const user = get(userState);
    return user.firstName + ' ' + user.lastName;
  },
  // set 함수의 두번쩨 매개변수는 inputHandler 이벤트 핸들러 함수에서 setUserName 함수에 전달된 값이 전달된다.
  set: ({ set }, name) => {
    const names = name.split(' ');

    // set 함수의 첫 번째 인자는 Selector가 사용하는 상태
    // 두 번째 매개변수는 새로운 값이다. 새로운 값은 업데이트 함수나 재설정 액션을 전파하는 DefalutValue 객체일 수 있다.
    // set의 두 번째 인자 이름을 prevState로 작명한 이유는 get 함수에서 받아온 atom의 값이 객체이기 때문에 기존의 값에서
    // 변경되지 않는 부분과 새롭게 바뀌 부분을 새로운 객체로 만들기 위해서이다. React의 상태는 항상 불변을 지켜야 하기때문이다.
    set(userState, prevState => ({
      ...prevState,
      firstName: names[0],
      lastName: names[1] || '',
    }));
  },
});

function App() {
  const [userName, setUserName] = useRecoilState(userNameSelector);
  const inputHandler = event => setUserName(event.target.value);

  return (
    <div>
      Full name: {userName}
      <br />
      <input type='text' onInput={inputHandler} />
    </div>
  );
}
```

<br />

## Recoil hooks

Atom과 Selector는 같은 인터페이스를 사용하여 상태를 조작할 수 있습니다. 이 부분은 Recoil의 강력한 장점이며 다른 상태 관리 라이브러리와 다르게 학습 곡선이 현저히 줄어 든다.

- `useRecoilState` : atom의 값을 구독하여 업데이트할 수 있는 hook. `useState`와 동일한 방식으로 사용할 수 있다. (selector에서도 사용할 수 있지만 get 함수만을 가지는 selector(`RecoilValueReadOnly`)는 사용할 수 없으며 set 함수를 포함한 selector만이 사용할 수 있다.)
- `useRecoilValue` : setter 함수 없이 atom의 값을 반환만 한다. get 함수를 가지는 selector에서 사용할 수 있다.
- `useSetRecoilState` : setter 함수만 반환하며 컴포넌트가 상태에 읽지 않고 쓰기만 하려고 할 때 추천한다.

<br />

## 참고 자료

[Recoil 공식문서](https://recoiljs.org/ko/)

[Recoil: 왕위를 계승하는 중입니다.](https://tv.naver.com/v/16970954?query=recoil&plClips=false:13490032:8077670:16970954:11558549:8077684:9483934:1102373:11595559:14078044:17581524:10035168:9484040:17173520:1100716:1089270:2912088:13927532:1102379:1159915:14253567)

[Recoil - 또 다른 React 상태 관리 라이브러리?](https://ui.toast.com/weekly-pick/ko_20200616)
