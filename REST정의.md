# REST API

<br />

## REST API의 탄생한 배경

<br />

> `a way provide interoperability between computer systems on the Internet.` (컴퓨터 시스템간의 상호 운영성을 제공하는 방법 중 하나)

### WEB(1991)

**어떻게 인터넷에서 정보를 공유할 것인가?**

- 모든 정보들을 하이퍼텍스트로 연결한다.
  - 표현 형식: HTML
  - 식별자: URI
  - 전송 방법: HTTP

```
HTML 형식으로 정보들을 표현하고 정보들의 식별자로 URI를 만들었고 그리고 그 정보들을 전송하는 방법으로 HTTP Protocol을 만들었다.
```

### REST 탄생

HTTP는 정보들을 전송하는 방식으로 HTTP/1.0 version이 나오기전에 이미 많은 사용자들이 WEB 전송 방식으로 급속도로 사용하고 있었습니다. 그러는 도중에 HTTP/1.0을 만들기 위해서 Roy T. Fielding이 제작에 참여하였습니다. 이 시점에서 HTTP를 정립하고 명세의 기능을 더하고 기존의 기능을 수정해야 하는 상황이 발생했습니다. 그러면 기존에 이미 구축된 WEB과 충돌하는 문제가 발생했습니다.

이전에 구축한 WEB을 망가트리지 않고 HTTP를 진보시킬 수 있는지 Roy T. Fielding은 고민을 했고 그 해결책으로 HTTP Object Model을 발표하게 되었습니다. 발표한 HTTP Object Model은 4년 후에 REST 논문을 Roy T. Fielding이 발표하면서 REST가 탄생하게 되었습니다.

위의 글을 정리해보면 Roy T. Fielding은 이전에 구축한 WEB에 영향을 미치지 않으면서 새롭게 구축한 HTTP/1.0 Protocol을 사용하는 방법을 만들기 위해서 REST를 만들게 되었다고 정리할 수 있습니다.

### REST란?

> `REST: Representational State Transfer`
>
> - Architectural Styles and the Design of Network-based Software Architectures

- REST API: REST 아키텍쳐 스타일을 따르는 API
- REST: 분산 하이퍼미디어 시스템(예: 웹)을 위한 아키텍쳐 스타일이면서 동시에 아키텍쳐 스타일의 집합
- 아키텍쳐 스타일: 제약조건들의 집합

### REST 구성하는 아키텍쳐 스타일

1. **Client - Server 구조** : REST 서버는 API 제공, 클라이언트는 사용자 인증이나 컨텍스트(세션, 로그인 정보 등)을 직접 관리하는 구조로 각각의 역할이 확실히 구분되기 때문에 클라이언트와 서버에서 개발해야 할 내용이 명확해지고 서로간 의존성이 줄어들게 된다.
2. **Stateless (무상태성)** : HTTP는 Stateless Protocol 이므로, REST 역시 무상태성을 갖는다. 즉, HttpSession과 같은 컨텍스트 저장소에 상태정보를 따로 저장하고 관리하지 않고, API 서버는 들어오는 요청만을 단순 처리하면 된다. 세션과 같은 컨텍스트 정보를 신경쓸 필요가 없어 구현이 단순해진다.
3. **Cacheable (캐시가능)** : HTTP 기존의 웹 표준을 그대로 사용하므로, 웹에서 사용하는 기존의 인프라를 그대로 활용 가능하다. HTTP 프로토콜 기반의 로드밸런서(mod_proxy)나, SSL은 물론이고 HTTP가 가진 가장 강력한 특징 중의 하나인 캐싱 기능을 적용할 수 있다. 일반적인 서비스에서 조회 기능이 주로 사용됨을 감안하면, HTTP 리소스들을 웹 캐쉬 서버 등에 캐싱하는 것은 용량이나 성능 면에서 이점이 있다. 캐싱 구현은 HTTP 프로토콜 표준에서 사용하는 Last-Modified 태그나 E-Tag를 이용하면 가능하다.
4. **Uniform Interface(유니폼 인터페이스)** : HTTP 표준에만 따른다면, 안드로이드/IOS 플랫폼이든, 특정 언어나 기술에 종속되지 않고 모든 플랫폼에 사용이 가능하며, URI로 지정한 리소스에 대한 조작이 가능한 아키텍처 스타일을 의미한다.
5. **계층형 구조** : API 서버는 순수 비지니스 로직을 수행하고, 그 앞단에 사용자 인증, 암호화(ssl), 로드밸런싱 등을 하는 계층을 추가하여 구조상의 유연상을 둘 수 있다. 이는 간단하게는 HA Proxy나 Apache의 Reverse Proxy를 통해, 더 나아가서는 API gateway 등을 활용하여 Micro Service Architecture로도 구현이 가능하게 한다.
6. **code-on-demand(optional)**:
7. **Self-descriptiveness (자체 표현 구조)** : 동사(Method) + 명사(URI) 로 이루어져있어 어떤 메서드에 무슨 행위를 하는지 알 수 있으며, 메시지 포맷 역시 JSON을 이용해서 직관적으로 이해가 가능한 구조로, REST API 메시지만 보고도 이를 쉽게 이해할 수 있다.

_대체로 REST API라고 불리는 API들은 어느정도의 REST를 잘 지키고 있습니다. 그 이유로는 우리가 정보 전송을 위해 사용하는 HTTP의 규칙만 잘 지켜도 Client - Server, Stateless, Cacheable, layered system와 같은 대부분의 REST Design Architecture을 지킬 수 있기 때문입니다. Code-on-demand 같은 경우는 optional로 serverd에서 코드를 Client 보내서 실행할 수 있어야 하며 가장 유명한 예시는 JavaScript를 server에서 Client로 보내는 방식입니다._

### Uniform Interface의 제약 조건

대부분의 REST 규칙은 HTTP의 규칙을 따른다고 해도 REST의 규칙 중 하나인 Uniform Interface는 외에는 전부 지킬 수 있습니다. 그렇다면 Uniform Interface를 지킬 수 있는 방법을 알아야 완벽한 REST API를 만들 수 있습니다.

- identification of resources
- manipulation of resources through representations
- **self-descriptive messages**
- **hypermedia as the engine of application state(HATEOAS)**

Uniform Interface의 제약 조건 중에서도 identification of resources와 manipulation of resources through representations는 잘 지켜지고 있습니다.

#### identification of resources

resources가 URI로 식별되어야 합니다.

#### manipulation of resources through representations

- representations 전송을 통해서 resources를 조작해야 합니다.
- resources를 만들거나 업데이트하고나 삭제하거나 할때 HTTP message에 표현을 담아서 전송을 해서 달성할 수 있어서 대체로 만족하고 있습니다.
