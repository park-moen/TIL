# WEB Component

<br />

## WHO Web Components?

WEB Component는 기능별로 재사용 가능한 코드 블록을 캡슐화하여 커스텀 엘리먼트를 생성하고 웹, 앱에서 활용할 수 있도록 해주는 다양한 기술들의 모음이라고 MDN에서 정의하고 있습니다. MDN 정의를 읽으면서 React에서 사용하는 Component 개념이 떠올랐다. 매우 비슷한 개념이지만 Web Component와 React Component는 부분 집합 관계로 이루어져 있습니다. WEB Component 개념에 React Component 개념이 포함되어 있습니다.

<img src="https://user-images.githubusercontent.com/57402711/113571559-928a8100-9651-11eb-8a61-912b9b7bbe5b.png" alt="image" style="zoom:67%;" />

​ [[wikipedia 부분 집합]](https://ko.wikipedia.org/wiki/%EC%97%AC%EC%A7%91%ED%95%A9)

```
컴포넌트는 독립적인 소프트웨어 모듈로써, 소프트웨어 시스템에서 독립적인 업무 또는 독립적인 기능을 수행하는 '모듈'로서 이후 시스템을 유지보수 하는데 있어 교체 가능한 부품입니다.
```

W3C에서 제한적인 HTML Element의 한계를 개선하기 위해서 Custom Element를 만드는 기술인 웹 컴포넌트 표준 및 명세를 만들었습니다. 웹 컴포넌트의 개념이 MDN에 비해서 조금 모호한 느낌을 받아서 간단한 예시를 보여 드리겠습니다.

```html
<div>{현재 시간}</div>
// HTML 엘리먼트에서는 현재 시간을 의미하는 엘리먼트가 존재하지 않습니다.
```

```html
<current-time
  >{현재 시간}<current-time>
    // 개발자가 직접 커스텀 엘리먼트를 만들어서 확실한 의미를 부여할 수
    있습니다.</current-time
  ></current-time
>
```

###

### HTML의 한계

- HTML 요소는 W3C에서 표준 및 명세가 있고 HTML 요소마다 의미가 있지만 브라우저와 운영체제에 따라 다른 동작으로 작동할 수 있습니다. 이 부분은 개발의 생산성 및 유지 보수 측면에 있어서 비효율적입니다.
- HTML5가 등장하면서 많은 요소들이 보충되었지만 이외에도 여러 기능을 지원하는 요소가 필요합니다.

_이러한 한계로 인해 Web Component라는 개념이 탄생하게 되었습니다._

### Web Component가 이슈화되고 있는 이유?

사실 웹 컴포넌트는 이전부터 존재했던 개념이였습니다. 2012년에 [웹 컴포넌트 - NAVER D2](https://d2.naver.com/helloworld/188655)에 대해서 상세한 설명을 하였습니다. 그렇다면 많은 프레임워크 컴포넌트가 존재하는데 Web Component가 이슈화되었을까요?

- 작은 서비스를 개발하는 경우 프레임워크 사용 자체가 오버 스팩이 될 수 있습니다.
- 프레임워크 및 라이브러리 별로 문법 및 개념을 학습해야하는 러닝 커브가 존재합니다.
- 서로 다른 프레임 워크를 통합해야 하는 경우 하나의 프레임워크를 선정해서 새로 개발을 진행해야 합니다. 즉, 상호응용성(Interoperability)이 발생합니다.
- 구글에서 웹 컴포넌트 스팩에 따라 구현한 ‘Google - Polymer’ 큰 주목을 받으면서 개발자들 사이에서 유행이 발생하게 되었습니다.

**‘Google I/O 2016’에서 ‘#UseThePlatform’ 키워드로 웹 표준에 대한 중요성을 각인시켜주었습니다.**

> - 프레임워크들은 다양한 문제를 해결할 강력한 도구이지만
> - 무거운 덩치는 앱을 무겁게 만들고
> - 리소스를 사용자에게 전가 시키며
> - 프레임워크 종속적인 코드를 생산한다
> - 그러한 문제들을 프레임워크 대신 브라우저 기능을 사용하여 해결하면
> - 프레임워크를 가볍게 만들어도 되고
> - 더 적은 자바스크립트 코드를 사용하게 되어
> - 표준 코드로 만든 성능 좋은 Awesome APP! 이 된다는 결론

_구글의 #UseThePlatform 키워드는 프레임워크로 무거운 앱을 만들지 말고 만드려는 기능을 표준대로 코딩하자는 의미를 내포하고 있는거 같습니다. 이러한 상황을 보면서 구글의 영향력이 개발자들 한테 매우 큰 바람을 일으키는거 같다는 느낌도 받게 되었습니다!!_

<br />

## Web Component 표준

### Custom Elements

- 기존의 HTML 요소 외에 사용자 인터페이스에서 원하는 대로 사용할 수 있는 사용자 정의 요소 및 해당 동작을 정의 할 수있는 JavaScript API 세트입니다.

- HTML5 엘리먼트에서 지원하지 않는 사용자 Custom Elements를 만들 수 있습니다.

  ```html
  <!DOCTYPE html>
  <html>
    <body>
      <div class="layout-container">
        <!-- html 문법에서는 의미를 가지지 않는 div태그로써 layout에 대한 정보를 명확하게 알 수 없습니다. -->
      </div>
    </body>
  </html>
  ```

  ```html
  <!DOCTYPE html>
  <html>
    <body>
      <layout-container>
        <!-- HTML5에 속한 Elemen에서는 div로 의미 없는 Element를 만들었지만 -->
        <!-- Custom Elements를 사용하면 의미가 정확한 Element를 만들 수 있습니다. -->
      </layout-container>
    </body>
  </html>
  ```

- HTML 엘리먼트와 JavaScript Class를 한몸으로 만들어서 Custom Elements의 Life Cycle을 조작할 수 있습니다.

  ```js
  class LayoutContainer extends HTMLElement {
    constructor() {
      // 클래스 초기화. 속성이나 하위 노드는 접근할 수는 없습니다.
      super();
    }

    static get observedAttributes() {
      // 모니터링 할 속성 이름
      return ['layout-container'];
    }

    connectedCallback() {
      // DOM에 추가되었다. 렌더링 등의 처리.
    }

    disconnectedCallback() {
      // DOM에서 제거되었다. 엘리먼트를 정리하.
    }

    attributeChangedCallback(attrName, oldVal, newVal) {
      // 속성이 추가/제거/변경되었다.
      // class에서의 this는 Custom Elements의 인스턴스를 가르키고 있습니다.
      this[attrName] = newVal;
    }
  }

  // <layout-container> 태그가 LayoutContainer 클래스를 사용하도록 연결하는 역할입니다..
  customElements.define('layout-container', LayoutContainer);
  ```

- HTML5에 속한 엘리먼트를 상속 받아서 Custom Elements를 만들 수 있습니다.

  ```js
  class LayoutContainer extends HTMLDivElement {
      ...
  }
  customElements.define('layout-container', LayoutContainer, {extends: 'div'});

  ```

- Custom Elements의 Tag 이름을 작성할 때는 예약어 및 업데이트 예정인 요소와 이름 충돌을 막기 위해서 케밥 표기법(-)을 사용해야 합니다.

### Shadow DOM

- HTML과 CSS의 스코프를 독립으로 다룰 수 있게 지원하는 개념 및 기능으로 프라이빗하게 유지할 수 있어, 다큐먼트의 다른 부분과의 충돌에 대한 걱정 없이 스크립트와 스타일을 작성할 수 있습니다.
- 서비스를 만들게 되면 HTML, CSS, JS 모두 하나의 글로벌 스코프라는 진입점이 존재합니다. 즉, 모든 부분을 open하는 Public한 점이 프론트 개발에 있어서 단점이 될 수 있습니다. (document.querySelector()로 모든 DOM tree에 접근할 수 있습니다.)
- Shadow DOM이 등장하기 전에는 `<iframe>`을 통해서 스코프를 독립적으로 관리할 수 있었지만 최선이 아니였습니다.
  - 의미없는 http 요청이 발생할 수 있습니다.
  - 별도의 페이지를 만들기때문에 perfomance 관점에서 매우 비효율적입니다.
  - iframe와 서비스의 도메인 주소가 동일하지 않을 경우 접근이 불가능합니다.
- 이러한 단점을 해결하기 위해서 Shadow DOM이 등장하게 되었습니다. 간단한 예시로 어떻게 Shadow DOM을 사용할 수 있는지 알아보겠습니다.

```js
const $publicContainer = document.createElement('div');

document.body.appendChild($publicContainer).innerHTML =
  '<style>div { background-color: #82b74b; }</style><div>Public Layout</div>';

const $privateContainer = document.createElement('div');

document.body
  .appendChild($privateContainer)
  .attachShadow({ mode: 'open' }).innerHTML =
  '<style>div { background-color: #ccc; }</style><div>Private Layout</div>';
```

<img src="https://user-images.githubusercontent.com/57402711/113661277-c3fe5d80-96e0-11eb-8584-a343a8af433e.png" alt="image" style="zoom:50%;" />

**`attachShadow({ mode: 'open' })`를 사용하여 글로벌 스코프에서 간단하게 분리할 수 있습니다.** 다른 방식으로는 슬롯 조합을 이용하는 방식도 존재합니다. 슬롯 조합은 `<slot name="blabla">` 을 사용하여 `<Element slot='blabla'>` 서로를 연결하는 조합입니다.

> - **쉐도우 돔**: 아래의 코드에서 h1, p등 **쉐도우 루트**에 붙어있는 DOM
> - **쉐도우 루트**: `#shadow-root`
> - **쉐도우 호스트**: **쉐도우 루트**의 부모. 아래의 코드에서 `div#slot-test`
> - **라이트 돔**: 도큐먼트의 **쉐도우 호스트**에 붙어있는 노드들. `span`

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta http-equiv="X-UA-Compatible" content="IE=edge" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Document</title>
  </head>
  <body>
    <div id="test-slot">
      <!-- Light DOM -->
      <span slot="title">Hello</span>
      <span slot="desc">world</span>
    </div>

    <script>
      // Shadow DOM
      document
        .querySelector('#test-slot')
        .attachShadow({ mode: 'open' }).innerHTML = `
          <h1>
            <slot name="title"></slot>
          </h1>
          <p>
            <slot name="desc"></slot>
          </p>
				`;
    </script>
  </body>
</html>
```

<img src="/Users/parkmoen/Library/Application Support/typora-user-images/image-20210406141618029.png" alt="image-20210406141618029" style="zoom:60%;" />

### HTML 템플릿

`<template>`과 `<slot>` 엘리먼트는 렌더링된 페이지에 나타나지 않는 마크업 템플릿을 작성할 수 있게 해줍니다. 그 후, 커스텀 엘리먼트의 구조를 기반으로 여러번 재사용할 수 있습니다.

- Template Element는 페이지를 불러온 순간 즉시 렌더링 하지 않지만, JS를 사용해 인스턴스를 생성할 수 있는 HTML 코드를 담을 수 있는 방법을 제공합니다.(content 프로퍼티에서 참조합니다.)
- Template Element가 등장하면서 자바스크립트 코드의 양을 줄일 수 있게 되었으며 조건에 따라서 DOM의 변경도 가능해졌습니다.(로그인/ 회원가입 페이지 변경 등)
- Template Element는 fragment를 활용해서 Virtual DOM을 만들어 content 프로퍼티로 접근하여 사용할 수 있습니다.
- 저의 추측이지만 Template Element의 content 프로퍼티는 DocumentFragment DOM Tree 자료구조를 참조하고 있습니다. 즉, 가상 돔 자체로 여러개의 DOM 자식들을 변경하게 되면 content 프로퍼티가 원하는 방향으로 동작하지 않는 현상이 발생하게 됩니다. **해결 방안으로 importNode(cloneNode, deep), cloneNode(deep) 메서드 사용을 통해서 DOM node를 복사하여 사용해야 합니다.**

```html
<section class="main-container">
  <!-- Template Element Area -->
</section>

<template id="list-container">
  <ul class="items">
    <li class="item">
      <!-- value -->
    </li>
  </ul>
</template>

<script>
  const $mainContainer = document.querySelector('.main-container');
  const $template = document.getElementById('list-container');
  const $clone = $template.importNode($template.content, true);
  const $li = $clone.querySelector('.item');

  $li.textContent = 'Hello Template Element';

  $mainContainer.appendChild($clone);
</script>
```

**Template Element와 Custom Elements를 컴포넌트로 구성하려면 HTML, JS 두 개의 파일이 필요하다는 단점으로 인해 현재 Template Element이 Web Component에서의 영향력이 떨어지고 있는 상태입니다.**

<br />

## 글을 마치며...

글을 마치기 전에 작성하지 않은 부분에 대해서 말씀드리겠습니다. W3C 표준 명세에서는 Web Component 스팩으로 3가지가 아닌 4가지가 공표하고 있습니다. HTML Import라는 하나의 표준이 더 존재하지만 제가 포스터에서 작성하지 않은 이유가 있습니다.

1. Firefox에서 현재 HTML Import를 지원하지 않고 있으며 다른 브라우저들도 구현에 나서지 않고 있기 때문입니다.
2. 위에서 말했던 Template Element의 단점인 HTML, JS 파일을 각각 필요한 단점을 해결하기 위해서 HTML Import가 탄생했지만 이미 ES Module표준이 존재하고 있으며, 많은 브라우저에서 지원 및 개발 중인 단계입니다.
3. 저의 생각이지만 HTML Import 사용을 위한 `<link>` 가 CSS 적용을 위한 `<link>` 유사하며, 차라리 EM Module을 사용하는 방식이 깔끔하다 고 생각합니다.
4. HTTP/2를 사용하면 여러 개 파일을 빠르게 받을 수 있다 해도, 여전히 번들링한 단일 파일을 받는 것이 빠릅니다.

이러한 단점으로 인해서 HTML Import는 MDN에서도 보이지 않고 있으며 다른 대책으로 Web Component를 만드는 현상이 발생하고 있습니다.(lit-html을 사용하는 경우가 많습니다).

현재 Web Component는 여러 충돌이 있었지만 빠르게 발전하고 있으며 Google에서는 polymer라는 라이브러리도 발표하며 많은 사람들에게 영향을 주고 있습니다. 10년 후에는 React, Vue, Angular 등의 프레임워크(라이브러리)를 사용하지 않으며, 순수 JS로 코딩이 하는 날이 오지 않을까 하는 저의 생각이 반영되어 블로그를 작성하게 되었습니다. (미리미리 준비하는 자세!!)

또한 현재 대부분의 개발은 React를 사용하고 있으며 React의 Virtual DOM 개념에 대해서 모호하게 알고 있다고 생각하고 있었습니다. 이번 Web Component 블로그를 작성하면서 React Component 및 Virtual DOM이 어떻게 만들어 지고 있는지에 대해서 생각을 정리할 수 있는 시간이었습니다.

<br />

## 참고 자료

[Web Components: Keep calm and UseThePlatform](https://ui.toast.com/weekly-pick/ko_20170428)

[Web Component in MDN](https://developer.mozilla.org/ko/docs/Web/Web_Components)

[Web Component](https://haeminmoon.github.io/2020/02/08/web-component/)

[웹 컴포넌트 (Web Component)](https://frontsom.tistory.com/5)
