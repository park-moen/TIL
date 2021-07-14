# Virtual DOM 의 필요성

## Virtual DOM은 무엇인가?

Virtual DOM(VDOM)은 UI의 이상적인 또는 "가상"적인 표현을 메모리에 저장하고 ReactDOM과 같은 라이브러리에 의해 "실제" DOM과 동기화 하는 프로그래밍 개념입니다.

이 접근방식이 React의 선언적 API를 가능하게 합니다. React에게 원하는 UI의 상태를 알려주면 DOM이 그 상태와 일치하도록 합니다. 이러한 방식은 앱 구축에 사용해야 하는 어트리뷰트 조작, 이벤트 처리, 수동 DOM 업데이트를 추상화합니다.

## Virtual DOM을 찾게 된 이유

DOM은 Element를 추가, 수정, 삭제와 같은 사용자 인터페이스 API를 JavaScript을 통해 지원하고 있으며, 이런 AP를 사용하면 Re-Render가 무수히 발생하는 결과를 초래합니다. 정적 웹을 만드는 경우에는 DOM API를 활용하여 개발을 해도 문제가 발생하지 않았지만 SPA, 대규모 어플리케이션 같은 프로젝트를 진행하면서 문제가 떠오르게 되었습니다.

- DOM 직접 조작하여 모든 Re-Render를 관리해야 하는 비효율적인 문제 발생
- SPA(Single Page Application) 기술을 활용한 어플리케이션 개발의 증가로 복잡한 DOM 조작에 따른 최적화 및 유지보수의 문제 발생
- Interactive web의 증가로 Layout의 변경 사항이 증가하며 Re-Render가 발생하는 문제 발생

즉, Re-Render에 관련된 이슈가 떠오르면서, Re-Render 이슈를 해결하기 위해서 Virtual DOM을 찾게 되었습니다.

**브라우저 렌더링 과정에 대해서 더 상세하게 공부하려면 여기를 클릭해주세요.**

## Virtual DOM

위의 Re-Render 문제의 해결책 중 하나의 방안으로 떠오르는 것이 Virtual DOM입니다. React의 재조정 개념을 통해서 어떻게 Virtual DOM을 사용하는지 설명하겠습니다.
