# REST API

<br />

## REST API의 탄생한 배경

<br />

> `a way provide interoperability between computer systems on the Internet.` (컴퓨터 시스템간의 상호 운영성을 제공하는 방법 중 하나)

<br />

## WEB(1991)

**어떻게 인터넷에서 정보를 공유할 것인가?**

- 모든 정보들을 하이퍼텍스트로 연결한다.
  - 표현 형식: HTML
  - 식별자: URI
  - 전송 방법: HTTP

```
HTML 형식으로 정보들을 표현하고 정보들의 식별자로 URI를 만들었고 그리고 그 정보들을 전송하는 방법으로 HTTP Protocol을 만들었다.
```

<br />

## REST 탄생

HTTP는 정보들을 전송하는 방식으로 HTTP/1.0 version이 나오기전에 이미 많은 사용자들이 WEB 전송 방식으로 급속도로 사용하고 있었습니다. 그러는 도중에 HTTP/1.0을 만들기 위해서 Roy T. Fielding이 제작에 참여하였습니다. 이 시점에서 HTTP를 정립하고 명세의 기능을 더하고 기존의 기능을 수정해야 하는 상황이 발생했습니다. 그러면 기존에 이미 구축된 WEB과 충돌하는 문제가 발생했습니다.

이전에 구축한 WEB을 망가트리지 않고 HTTP를 진보시킬 수 있는지 Roy T. Fielding은 고민을 했고 그 해결책으로 HTTP Object Model을 발표하게 되었습니다. 발표한 HTTP Object Model은 4년 후에 REST 논문을 Roy T. Fielding이 발표하면서 REST가 탄생하게 되었습니다.

위의 글을 정리해보면 Roy T. Fielding은 이전에 구축한 WEB에 영향을 미치지 않으면서 새롭게 구축한 HTTP/1.0 Protocol을 사용하는 방법을 만들기 위해서 REST를 만들게 되었다고 정리할 수 있습니다.

<br />

## REST란?

> `REST: Representational State Transfer`
>
> - Architectural Styles and the Design of Network-based Software Architectures

- REST API: REST 아키텍쳐 스타일을 따르는 API
- REST: 분산 하이퍼미디어 시스템(예: 웹)을 위한 아키텍쳐 스타일이면서 동시에 아키텍쳐 스타일의 집합
- 아키텍쳐 스타일: 제약조건들의 집합

REST는 제약 조건의 집합을 의미하며 모든 제약조건을 지켜야 REST라고 지칭할 수 있는 점을 주의해야합니다. 그러기 위해서는 REST에서 사용하는 제약 조건에 대해서 알아보겠습니다.

<br />

## REST 구성하는 아키텍쳐 스타일

1. **Client - Server 구조** : REST 서버는 API 제공, 클라이언트는 사용자 인증이나 컨텍스트(세션, 로그인 정보 등)을 직접 관리하는 구조로 각각의 역할이 확실히 구분되기 때문에 클라이언트와 서버에서 개발해야 할 내용이 명확해지고 서로간 의존성이 줄어들게 된다.
2. **Stateless (무상태성)** : HTTP는 Stateless Protocol 이므로, REST 역시 무상태성을 갖는다. 즉, HttpSession과 같은 컨텍스트 저장소에 상태정보를 따로 저장하고 관리하지 않고, API 서버는 들어오는 요청만을 단순 처리하면 된다. 세션과 같은 컨텍스트 정보를 신경쓸 필요가 없어 구현이 단순해진다.
3. **Cacheable (캐시가능)** : HTTP 기존의 웹 표준을 그대로 사용하므로, 웹에서 사용하는 기존의 인프라를 그대로 활용 가능하다. HTTP 프로토콜 기반의 로드밸런서(mod_proxy)나, SSL은 물론이고 HTTP가 가진 가장 강력한 특징 중의 하나인 캐싱 기능을 적용할 수 있다. 일반적인 서비스에서 조회 기능이 주로 사용됨을 감안하면, HTTP 리소스들을 웹 캐쉬 서버 등에 캐싱하는 것은 용량이나 성능 면에서 이점이 있다. 캐싱 구현은 HTTP 프로토콜 표준에서 사용하는 Last-Modified 태그나 E-Tag를 이용하면 가능하다.
4. **Uniform Interface(유니폼 인터페이스)** : HTTP 표준에만 따른다면, 안드로이드/IOS 플랫폼이든, 특정 언어나 기술에 종속되지 않고 모든 플랫폼에 사용이 가능하며, URI로 지정한 리소스에 대한 조작이 가능한 아키텍처 스타일을 의미한다.
5. **계층형 구조** : API 서버는 순수 비지니스 로직을 수행하고, 그 앞단에 사용자 인증, 암호화(ssl), 로드밸런싱 등을 하는 계층을 추가하여 구조상의 유연상을 둘 수 있다. 이는 간단하게는 HA Proxy나 Apache의 Reverse Proxy를 통해, 더 나아가서는 API gateway 등을 활용하여 Micro Service Architecture로도 구현이 가능하게 한다.
6. **code-on-demand(optional)**:
7. **Self-descriptiveness (자체 표현 구조)** : 동사(Method) + 명사(URI) 로 이루어져있어 어떤 메서드에 무슨 행위를 하는지 알 수 있으며, 메시지 포맷 역시 JSON을 이용해서 직관적으로 이해가 가능한 구조로, REST API 메시지만 보고도 이를 쉽게 이해할 수 있다.

_대체로 REST API라고 불리는 API들은 어느정도의 REST를 잘 지키고 있습니다. 그 이유로는 우리가 정보 전송을 위해 사용하는 HTTP의 규칙만 잘 지켜도 Client - Server, Stateless, Cacheable, layered system와 같은 대부분의 REST Design Architecture을 지킬 수 있기 때문입니다. Code-on-demand 같은 경우는 optional로 serverd에서 코드를 Client 보내서 실행할 수 있어야 하며 가장 유명한 예시는 JavaScript를 server에서 Client로 보내는 방식입니다._

<br />

## Uniform Interface의 제약 조건

대부분의 REST 규칙은 HTTP의 규칙을 따른다고 해도 REST의 규칙 중 하나인 Uniform Interface는 외에는 전부 지킬 수 있습니다. 그렇다면 Uniform Interface를 지킬 수 있는 방법을 알아야 완벽한 REST API를 만들 수 있습니다.

- identification of resources
- manipulation of resources through representations
- **self-descriptive messages**
- **hypermedia as the engine of application state(HATEOAS)**

Uniform Interface의 제약 조건 중에서도 identification of resources와 manipulation of resources through representations는 잘 지켜지고 있습니다.

### identification of resources

- resources가 URI로 식별되어야 합니다.
- 정확하게 말하면 리소스를 식별하는 방법이 동일해야 합니다. 대부분의 API는 URI로 resources를 식별하는 경우가 많습니다. 그렇기때문에 대부분의 API는 Uniform Interface의 제약 조건 중 첫번째 제약 조건을 쉽게 만족할 수 있습니다.

```
URL로 통일해서 통신을 합니다.

http://localhost:8080/profile-photo
http://localhost:8080/location
```

### manipulation of resources through representations

- representations 전송을 통해서 resources를 조작해야 합니다.
- resources를 만들거나 업데이트하고나 삭제하거나 할때 HTTP message에 표현을 담아서 전송을 해서 달성할 수 있어서 대체로 만족하고 있습니다.
- 동일한 URI에서 동일한 자원의 표현을 둘 이상 지원할 수 있습니다.
- representations의 형태는 content-type으로 결정합니다.

### **self-descriptive messages (자체 표현 구조)**

**자기 서술적 메시지라는 의미로 각 메시지는 자신을 어떻게 처리해야 하는지 충분한 정보를 HTTP Method, Status Code, Header 등을 활용하여 전달 해야합니다.**

```http
GET / HTTP/1.1

이 HTTP 요청 메시지는 목적지가 존제하지 않기때문에 self-descriptive가 아닙니다.
```

```http
GET / HTTP/1.1
Host: www.example.org

목적지를 추가해서 self-descriptive 조건을 만족 시킬 수 있습니다.
```

```http
HTTP/1.1 200 OK
	[ { “op” : “remove”, “path” : “a/b/c"} ]

이 부분을 해석하기 위해서는 Client가 이 응답을 받고 해석해야 하지만 이 정보가 어떤 문법으로 작성된것인지 알 수 없기때문에 self-descriptive가 아닙니다.
```

```http
HTTP/1.1 200 OK
Content-Type: application/json
	[ { “op” : “remove”, “path” : “a/b/c"} ]

Content-Type에 어떻게 문법을 해석할것인지를 알려주면 대갈호의 의미, 중갈호의 의미, 큰 따옴표의 의미 등을 Client가 문법에 맞게 파싱이 가능해지고 문법을 해석할 수 있게 만들어 줍니다.

여기까지 작성하여도 self-descriptive의 조건을 만족하지 않습니다. 왜냐하면 op, path 와 같은 문자열로 보내는 데이터의 정확한 의미를 알 수 없기때문입니다.
```

```http
HTTP/1.1 200 OK
Content-Type: application/json + patch+json
	[ { “op” : “remove”, “path” : “a/b/c"} ]

patch+json은 media type으로 정의되어 있는 메시지로 JSON PATCH 명세를 찾아서 메시지를 해석할 수 있습니다. 이제는 self-descriptive 조건을 만족하고 있습니다.
```

self-descriptive messages는 메세지를 봤을때 메시지의 내용으로 온전히 모든 정보가 해석이 가능해야 하지만 대부분의 API가 이 부분을 상당히 만족하지 못하고 있습니다.

### Hypermedia As The Engine Of Application State(HATEOAS)

- 애플리케이션의 상태는 Hyperlink를 이용해서 전이되어야합니다.
- Hypermedia는 HyperText를 확장한 개념으로 hypertext가 아닌 매체를 고려해서 상위 개념으로 정의했습니다. 대부분의 API는 hypertext로 표현합니다.
- Server에서 응답을 보낼때는 Hpyermedia를 데이터와 함께 보내서 다음 액션에 대한 선택지를 클라이언트에게 줘야합니다.
- 클라이언트는 서버의 독자적인 진화에 영향을 받아선 안되며, 서버 상의 구현에 의존하면 안됩니다.

**결국 HATEOAS의 목적은(서버-클라이언트 간 의존성을 분리해야만 가능한) 독자적인 진화와 확장을 보조하는 것이며, hypermedia는 그 목적을 이루는 데 기여해야 합니다.**

```http
HTTP/1.1 200 OK
Content-Type: text/html
<html>
<head></head>
<body>
	<a href="/test">test</a>
</body>
</html>

server의 응답이 html 같은 경우는 <a> 태그를 통해서 hyperLink가 있고 hyperLink로 다음 상태로 전이가 가능해서 따로 설정을 하지 않아도 HATEOAS를 만족합니다.
```

```http
HTTP/1.1 200 OK
Content-Type: application/json
Link: </articles/1>; rel=“previous”,
      </articles/3>; rel=“next”;
{
    “Title” : “The second article”,
    “content” : “Hello! Brother"
}

JSON data 또한 Link 헤더를 사용하여 hyperLikn를 통해서 리스소와 연결되어 있는 다른 리소스를 가르킬 수 있는 기능을 제공해서 HATEOAS를 만족할 수 있습니다.

위의 http를 보면 게시글에 관련된 정보로 이전 게시물의 URI는 </articles/1>이고 다음 게시물의 URI는 </articles/3>라고 하는 정보를 표현해 주었습니다. 또한 이 정보는 Link 헤더가 http 표준으로 이미 정의되어 있기 때문에 이 메시지를 보는 사람이 온전히 해석해서 어떻게 링크가 되어 있는지가를 이해하고 hyperLink를 찾아 다른 상태로 전이를 할 수 있어서 위의 예제는 HATEOAS의 조건을 만족합니다.
```

### 왜 Uniform Interface가 필요한가?

- 서버와 클라이언트가 각각 독립적인 진화를 이루기 위해서입니다.
- **서버의 기능이 변경되어도 클라이언트를 업데이트할 필요가 없어야 하기 때문입니다. **
- REST를 만들게 된 계기를 생각해보면 HTTP/1.0를 만들게 되면서 이전에 구축한 WEB에 영향이 미치지 않게 하기 위해서 REST가 등장했습니다. 즉, 독립적인 진화를 통해 상호 운영성(처음 REST 개념에서 이해할 수 없는 용어)을 지키는 방법을 고안한 것이 REST입니다.

**REST가 목적하는 것은 독립적인 진화입니다. 독립적인 진화를 하기 위해서는 Uniform Interface가 반드시 만족되어야 합니다. 그렇기 때문에 Uniform Interface를 만족하지 못하면 REST라고 부를 수 없습니다.**

<br />

## 참고 자료

---

[[REST API]REST를 사용할 때 주의해야할 점](https://sabarada.tistory.com/9)

[REST - 논문(요약) 훑어보기](http://haah.kr/2017/05/24/rest-the-dissertation-summary/)

[REST의 representation이란 무엇인가](https://blog.npcode.com/2017/04/03/rest%ec%9d%98-representation%ec%9d%b4%eb%9e%80-%eb%ac%b4%ec%97%87%ec%9d%b8%ea%b0%80/)

[그런 rest api로 괜찮은가](https://www.youtube.com/watch?v=RP_f5dMoHFc)
