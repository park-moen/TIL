# JavaScript에서 Currying Pattern 적용하기

---

JavaScript에서의 function의 역활은 재사용성 및 유지보수에서 많이 사용됩니다. 또한 자바스크립트는 유연하게 함수형 프로그래밍 또는 객체 지향 프로그래밍을 사용하여 로직을 작성할 수 있습니다. 이 글은 JavaScript의 함수형 프로그래밍으로 로직을 작성할 때 함수와 함께 사용하는 Curry과 Partial Application 기술에 대해서 설명하겠습니다.

## Currying

---

### Currying 정의

- 컴퓨터 과학에서 커링이란 다중 인수를 가지는 함수를 단일 인수를 가지는 함수들의 함수열로 바꾸는 기술입니다.
- 조금 더 쉽게 말해서는 `fucntion temp(a, b, c)` 다중 인수를 가지는 temp 함수를 `function(a)(b)(c)` 처럼 각각의 인수가 호출 가능한 함수 프로세스로 변환하는 기술입니다.

### Currying의 특징

- 커링은 하나의 인수만을 받아서 결과를 리턴합니다. 커링의 정의처럼 다중 인수를 가지는 함수를 단일 인수를 가지는 함수를 변환한다는 부분과 일치하네요.
- 클로저를 사용하여 내부 함수가 외부 함수보다 생명주기가 길어서 외부 함수의 식별자를 식별할 수 있습니다. (이 부분은 뭐... 함수형 프로그래밍의 특징이 맞을 수도 있겟지만 Currying 기법을 사용하기 위해서는 클로저의 개념도 알아야 하기때문에 Currying의 특징에 넣어 보았습니다.)
- JavaScript의 function은 일급 객체로서 인수로 function을 사용할 수 있습니다.(HOF/ 고차 함수) Curry function을 만들어서 curry function의 인수로 function을 넣어서 Currying 기법을 사용할 수 있는 패턴도 존재합니다.
- 인자에 따라서 function의 기능 확장을 할 수 있습니다.

**즉, Currying 기법을 사용하기 전에 클로저와 HOF를 미리 숙지해야 능숙하게 Currying을 사용할 수 있습니다.**

### 간단한 Currying 예시 및 Curry polyfill

```js
// Normal function
const add = (x, y, z) => x + y + z;

add(1, 2, 3);

// arrow function
const add = x => y => z => x + y + z;

add(1)(2)(3);

const addX = add(1);
const addY = addX(2);
const res = addY(3);

console.log(res);


// Curry polyfill

function multiply(a, b, c) {
  return a * b * c;
}

function curry(func) {
  return function curried(...args) {
    if (args.length >= func.length) {
      return func.apply(this, args);
    } else {
      return function(...args2) {
        return args.apply(this, args.concat(args2));
      };
    }
  };
}

const curried = curry(multiply);
curried(1, ,2, 3);
curried(1)(2, 3);
curried(1)(2)(3);

```
