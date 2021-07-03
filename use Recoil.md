# Recoil 사용하기

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

```react
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

## RecoilRoot

recoil을 사용하기 위해서는 다른 상태 관리 라이브러리(redux, mobx)처럼 초기 설정을 해야한다. 다른 라이브러리와 차이점으로는 휠씬 간결한 초기 설정으로 쉽게 사용할 수 있다는 장점이 존재한다.

redux 라이브로리를 초기 설정하기 위해서는 store를 세팅하고 컴포넌트의 최상단에 provider를 선언 후 provider와 store를 연결해야 하듯이 recoil 또한 최상단의 컴포넌트(상태 관리 라이브러리는 사용할 최상단 루트, 루트 컴포넌트)에 RecoilRoot를 넣어준다.

```react
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

## Atom

Atom은 상태의 단위이며, React의 state(상태)와 동일한 의미로 사용되고 있다. 동일한 atom이 여러 컴포넌트에서 사용되는 경우 모든 컴포넌트는 상태를 공유한다. 즉 Atom을 공유하고 있는 컴포넌트는 Atom이 변경되면 리랜더링이 된다.(atom의 값을 읽는 컴포넌트들은 암묵적으로 atom을 **구독**한다.) Atom을 생성하기 위해서는 Recoil의 atom 함수를 사용하며 함수의 인자로는 객체를 넘겨 준다.(필수로 key, default를 작성해야 한다.)

- key: atom 상태를 식별하는 고유한 상태로 두개의 atom이 동일한 key를 갖는 것은 오류를 발생시키는 원인이 되기 때문에 key 문자열은 전역적으로 고유해야 한다.
- default: React state처럼 기본 값을 설정하는 부분으로 JS의 모든 타입이 올 수 있다.

```react
import React from 'react';
import { atom, useRecoilState } from 'recoil';

const countState = atom({
  key: 'countState',
  default: 0,
});

const inputState = atom({
  key:'inputState',
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
  )
}

// 간단한 App을 제작할때는 컴포넌트에 atom을 선언해도 되지만 규모가 큰 App을 제작할때는 공용으로 사용하는
// 상태를 파일로 분리하는 것을 추천한다.
```

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
  - set : 업스트림(비동기) Recoil 상태의 값을 설정할 때 사용되는 함수. 첫 번째 매개변수는 Recoil 상태, 두 번째 매개변수는 새로운 값이다.
