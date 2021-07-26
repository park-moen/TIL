# REST API 와 GraphQL

<br/>

## REST란?

REST는 “Representational State Transfer”의 약자로 자원(Resource)과 상태(State)를 주고 받는 모든 행위를 의미합니다. REST는 프로토콜이나 표준이 아닌 네트워크 아키텍처 원칙입니다. API를 개발하기 위해 무조건적으로 REST 원칙을 따를 필요는 없습니다.

1. 자원(Resource): 넓은 의미에서는 해당 소프트웨어가 관리하는 모든 것이 될 수 있고 해당 소프트웨어 자체가 자원이 될 수 있습니다. HTTP 요청의 대상을 자원이라고 부르며 HTTP 통신에서는 URI를 자원으로 사용합니다.

   - representation은 리소스의 특정 시점의 상태를 반영하고 있는 정보이며 representation data와 representation metadata를 통해 특정 시점에 상태를 반영한 정보입니다.

   - 자원에는 Representations(표현)이 포함되어 있으며 표현은 자원의 표현하기 위한 식별자 또는 이름으로 지칭합니다.
   - 예로 보면 집이라는 자원이 있고 집의 종류가 여러가지가 있으며 집의 종류들을 표현하기 위해서는 아파트, 주택 등의 이름으로 자원을 식별하는 식별자 또는 이름을 지정하여 필요한 자원에 접근할 수 있습니다.

2. 상태(State): REST에서 리소스의 상태와 애플리케이션의 상태(State)의 동일한 이름을 사용하지만 본질적으로 다른 의미입니다.

   - 애플리케이션에서의 State는 웹 애플리케이션의 상태를 의미합니다. 예를 들면 A페이지에서 B페이지로 이동하는 링크를 클릭하여 B페이지로 이동하게 됩니다. 여기서 상태는 A페이지에서 B페이지로 바뀐 상태를 애플리케이션에서의 상태로 지칭합니다.

   - 리소스에서의 State는 representation(표현)로써 특정 시점의 상태를 반영하고 있는 정보이를 의미합니다.
   - 애플리케이션의 상태에서 상태 전달하기 위해서는 전달 시점과 전송 방식에 대해서 알고 있어야 합니다,
     1. 전송 시점: 데이터를 요청하는 시점에 자원의 상태(표현)을 전달합니다.
     2. 전송 방식: 대부분은 컴퓨터와 사람이 모두 이해할 수 있는 JSON을 사용하지만 JSON 이전에는 XML을 사용했습니다.

<br/>

## REST의 실사용

**HTTP 통신(URI)을 통해 Resource를 URI에 명시하고 HTTP Method를 통해 해당 자원에 대한 CRUD 조작을 적용하는 것을 의미합니다.**

- 자원을 식별할 수 있는 ID를 HTTP 통신에서 사용되는 URI에 부여 전송합니다.
- CRUD 조작은 HTTP Method(GET, POST, PUT, DELETE)를 사용합니다.

<br/>

## REST 구성 요소

1. 자원(Resource)

   - URI를 통해서 자원을 가져올 수 있습니다.
   - URI은 해당 사이트의 자원을 나타내는 유일한 출처입니다.
   - naver.com/user or naver.com/login 등의 예제가 있으며 login, user등을 통해서 자원을 취득할 수 있습니다.
   - 클라이언트는 URI을 통해서 특정 자원을 지정하고 해당 자원에 대한 조작을 서버에 요청합니다.

2. 행워(Method)
   - HTTP 프로토콜 Method를 사용합니다.
3. 표현(payload or representation of resource)
   - REST 아키텍처에서 자원을 JSON, XML, TEXT 등 여러 형태의 representation으로 나타낼 수 있습니다.
   - 현재는 JSON을 통해 데이터를 통신합니다.

HTTP Method

|           CRUD            | HTTP Method |   Route   | Payload |
| :-----------------------: | :---------: | :-------: | :-----: |
|  Resource들의 목록 취득   |     GET     |   /user   |    x    |
| 특정 Resource의 목록 취득 |     GET     | /user/:id |    o    |
|       Resource 생성       |    POST     |   /user   |    o    |
|    Resource 전체 교체     |     PUT     | /user/:id |    o    |
|       Resource 수정       |    PATCH    | /user/:id |    o    |
|       Resource 삭제       |   DELETE    | /user/:id |    x    |

<br/>

## REST의 특징

1.  **Uniform (유니폼 인터페이스)** : HTTP 표준에만 따른다면, 안드로이드/IOS 플랫폼이든, 특정 언어나 기술에 종속되지 않고 모든 플랫폼에 사용이 가능하며, URI로 지정한 리소스에 대한 조작이 가능한 아키텍처 스타일을 의미한다.
2.  **Stateless (무상태성)** : HTTP는 Stateless Protocol 이므로, REST 역시 무상태성을 갖는다. 즉, HttpSession과 같은 컨텍스트 저장소에 상태정보를 따로 저장하고 관리하지 않고, API 서버는 들어오는 요청만을 단순 처리하면 된다. 세션과 같은 컨텍스트 정보를 신경쓸 필요가 없어 구현이 단순해진다.
3.  **Cacheable (캐시가능)** : HTTP 기존의 웹 표준을 그대로 사용하므로, 웹에서 사용하는 기존의 인프라를 그대로 활용 가능하다. HTTP 프로토콜 기반의 로드밸런서(mod_proxy)나, SSL은 물론이고 HTTP가 가진 가장 강력한 특징 중의 하나인 캐싱 기능을 적용할 수 있다. 일반적인 서비스에서 조회 기능이 주로 사용됨을 감안하면, HTTP 리소스들을 웹 캐쉬 서버 등에 캐싱하는 것은 용량이나 성능 면에서 이점이 있다. 캐싱 구현은 HTTP 프로토콜 표준에서 사용하는 Last-Modified 태그나 E-Tag를 이용하면 가능하다.
4.  **Self-descriptiveness (자체 표현 구조)** : 동사(Method) + 명사(URI) 로 이루어져있어 어떤 메서드에 무슨 행위를 하는지 알 수 있으며, 메시지 포맷 역시 JSON을 이용해서 직관적으로 이해가 가능한 구조로, REST API 메시지만 보고도 이를 쉽게 이해할 수 있다.
5.  **Client - Server 구조** : REST 서버는 API 제공, 클라이언트는 사용자 인증이나 컨텍스트(세션, 로그인 정보 등)을 직접 관리하는 구조로 각각의 역할이 확실히 구분되기 때문에 클라이언트와 서버에서 개발해야 할 내용이 명확해지고 서로간 의존성이 줄어들게 된다.
6.  **계층형 구조** : API 서버는 순수 비지니스 로직을 수행하고, 그 앞단에 사용자 인증, 암호화(ssl), 로드밸런싱 등을 하는 계층을 추가하여 구조상의 유연상을 둘 수 있다. 이는 간단하게는 HA Proxy나 Apache의 Reverse Proxy를 통해, 더 나아가서는 API gateway 등을 활용하여 Micro Service Architecture로도 구현이 가능하게 한다.

<br/>

## REST API란?

- API(Application Programming Interface): API는 사용자가 원하는 것을 시스템에 전달할 수 있게 지원하여 시스템이 요청을 이해하고 이행할 수 있도록 할 수 있는 클라이언트와 서버의 조정자입니다.
- REST API는 REST 아키텍처를 기반으로 API를 설계한 소프트웨어입니다.

### REST API 기본 설계

1. URI는 정보의 자원을 표현해야 합니다.
   - `GET /user` `GET /house` 등으로 식별할 수 있는 자원으로 표현합니다.
2. 자원에 대한 행위는 [HTTP Method](#REST 구성 요소) (GET, POST, PUT, DELETE 등)으로 표현합니다.

### REST API 설계시 주의 사항

1. 슬래시 구분자(/)는 계층 관계를 나타내는데 사용합니다.
2. URI 마지막 문자로 슬래시(/)를 포함하지 않습니다.

- 혼돈을 주지 않기 위해서 REST API를 설계하기 위해서는 마지막 (/)를 반드시 포함하지 않아야 합니다.

3. 하이픈(-)은 URI 가독성을 높이는데 사용합니다,
   - 만약 자원의 표현이 길어지면 하이폰(-)을 사용해서 단어 두개를 끊어줄 수 있습니다.
   - 문자가 가려지는 현상 및 가독성을 위해서 밑줄(\_)은 URI에 사용하지 않습니다.
4. 대문자 대신 소문자를 사용합니다.
   - 3번 주의 사항처럼 자원의 표현이 길어지면 카멜케이스로 구분하지 않고 하이폰(-)인 파스칼 케이스를 사용합니다.
5. 파일확장자는 URI에 포함하지 않습니다.
   - Accept Header를 통해서 파일 확장자를 설정합니다.

<br/>

## RESTfull

### RESTfull이란 ?

- RESTfull는 REST 아키텍처로 API를 설계하여 웹서비스에 사용되는 언어입니다.
- REST API를 제공하는 웹 서비스를 RESTfull한 서비스라고 지칭합니다. (REST 원칙을 따른 시스템을 RESTfull하다고 생각합니다.)
- RESTfull은 REST를 REST답게 쓰기 위한 방법론으로 REST의 표준 지침은 아닙니다.

### RESTfull의 목적

- 누구나 이해할 수 있으며 사용하기 쉬운 REST API를 설계하기 위해서입니다.
- RESTfull의 설계 목적은 성능 향상을 위한 설계라기 보다 일관적인 컨벤션을 통한 누구나 이해할 수 있고 사용할 수 있으며 어디서나 호환이 가능한 API를 만들기 위해서입니다. (성능을 위해서는 RESTfull 한 API를 구현하지 않아도 됩니다.)

### RESTfull을 따르지 않는 API

- [REST API 설계 규칙](#REST API 설계시 주의 사항) 을 따르지 않고 CRUD 표현을 POST 방식으로만 사용하는 경우
- `GET /USER/`- 대문자 대신 소문자를 사용해야 하며 마지막 자원의 표현에는 (/)를 작성하지 않습니다.
- `GET /show_user` - 자원의 표현은 HTTP Method로 표현하며 밑줄(\_) 대신에 하이폰(-)을 사용해야 합니다.

<br/>

## REST API의 한계

- EndPonint에 의존하여 관련된 내용만을 관리합니다.
- 기능이 필요할 경우마다 API를 매번 생성하기 때문에 over-fetching, under-fetching이 발생합니다.
  - over-fetching: endpoint 별로 사용하지 않는 데이터를 너무 많이 가져오는 현상
  - under-fetching: 충분한 데이터를 가져오지 못해서 새로운 endpoint를 호출
- over-fetching, under-fetching로 인한 복잡한 로직이 발생할 수 있습니다.
- Resource별로 고정된 Endpont를 가지고 있기때문에 비슷한 데이터라도 새로 API를 생성해야합니다.(여러개의 endpoint가 존재)

```
GET /user
// user에서 name과 age만을 필요로 하지만 user에 있는 모든 데이터를 받아오는 over-fetching이 발생


GET /user/1
GET /item
GET /total-price
// 여러 데이터를 받아와야하는 경우 endpoint를 여러번 호출하는 under-fetching이 발생
```

<br/>

## GraphQL

### GraphQL이란?

- REST API의 한계를 극복하기 위해서 facebook에서 만든 Graph Query Language입니다.
- Query Language 는 정보를 얻기 위해 보내는 질의문(Query)을 만들기 위해 사용되는 Computer언어의 일종입니다.
- GraphQL은 하나의 Endpoint만 존재합니다.
- GraphQL API 는 요청할 때 사용한 Query 문에 따라 응답의 구조가 달라집니다.(유연하게 요청할 수 있습니다.)

### GraphQL의 효율성

- over-fetching, under-fetching 발생을 억제할 수 있습니다.
- 원하는 정보를 Query 문을 사용해서 유연하게 가져올 수 있습니다.
- HTTP 요청의 횟수를 줄일 수 있습니다.
  - Endpoint가 하나이기 떼문에 원하는 정보만을 Query 문에 작성하여 데이터를 받아 올 수 있습니다.
- HTTP 응답의 Size 를 줄일 수 있습니다.
  - 불필요한 정보를 받아오지 않을 수 있습니다.

```
// user에서 name과 age만을 Query문을 통해서 받아 올 수 있습니다.
query {
	user {
		user_name,
		user_age
	}
}

// 여러 데이터를 EndPoint에 종속되지 않고 받아 올 수 있으며, 필요한 정보를 유연하게 받아 올 수 있습니다.
query {
	user(user_id: 1) {
		user_name,
		user_address
	}
	item {
		item_name,
		item_price
	}
	total-info {
		total_price,
		total_count
	}
}
```

### GraphQL의 단점

- 고정된 요청과 응답만 필요할 경우에는 Query 로 인해 요청의 크기가 RESTful API 의 경우보다 더 커집니다.
- File 전송 등 Text 전송만으로 처리하기 힘든 내용의 처리가 어렵습니다.
- 백엔드, 프론트엔드 양쪽 다 학습 곡선이 있습니다.
- 캐싱 기능이 어렵습니다.
  - 현재는 대부분의 언어에서 라이브러리를 지원합니다.(ex: apollo)

<br/>

## REST API vs GraphQL

1. REST API
   - HTTP 와 HTTPs 에 의한 Caching 처리를 원할때 사용할 수 있습니다.
   - File 전송 등 단순한 Text 로 처리되지 않는 요청들이 있을 때
   - 고정적인 데이터를 받기를 원할 때 유용합니다.
2. GraphQL
   - 여러개의 EndPoint에서 요청을 받아 오는 경우에 유용합니다.
   - 대부분의 CRUD 작업을 처리할때 유용합니다.
   - 유동적인 데이터를 받아올때 유용합니다.

**즉, REST API와 GraphQL의 특징을 정확히 이해하고 상황별로 사용해야합니다.**

<br/>

## 참고 자료

[[Netwok] REST란? REST API란? RESTful이란?](https://gmlwjd9405.github.io/2018/09/21/rest-and-restful.html)

[REST의 representation이란 무엇인가](https://blog.npcode.com/2017/04/03/rest%ec%9d%98-representation%ec%9d%b4%eb%9e%80-%eb%ac%b4%ec%97%87%ec%9d%b8%ea%b0%80/)

[REST API(RESTful API, 레스트풀 API)란? 구현 및 사용법](https://www.redhat.com/ko/topics/api/what-is-a-rest-api)

[그런 rest api로 괜찮은가](https://www.youtube.com/watch?v=RP_f5dMoHFc)

[RESTful API란 ?](https://brainbackdoor.tistory.com/53)

[GraphQL과 RESTful API](https://www.holaxprogramming.com/2018/01/20/graphql-vs-restful-api/)

[GraphQL이란?](https://brownbears.tistory.com/450)
