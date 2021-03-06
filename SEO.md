# 검색엔진 최적화(SEO)

<br/>

## 검색 엔진 최적화(SEO)에 필요한 용어 정리

`색인(indexing)` : **검색을 빠르게 하기 위해서 항목별로 데이터를 저장하는 장소** 를 지칭하며 색인 과정이 존재하지 않게 되면 필요한 데이터를 찾기 위해 검색 엔진이 모든 데이터를 찾아야하는 문제가 발생합니다.

예를 들어 도서관에서 소설, 수필 등으로 나누는 과정을 통해서 원하는 서적을 찾는 방식처럼 **특정 항목별로 데이털르 저장하는 과정을 색인(indexing)**이라고 합니다.

`크롤링` : 웹 페이지를 가져와서 데이터를 추출하는 행위로 구글에서는 링크를 따라가거나, 사이드맵을 읽거나, 다른 여러 방법으로 URL을 찾아냅니다. 즉, 필요한 정보를 찾는 작업으로 인덱싱하기 위해 사전에 필요한 작업입니다.

예: Google은 신규 또는 업데이트된 웹을 크롤링하여 페이지를 찾은 다음 필요에 따라 색인(indexing)을 생성합니다.

`크롤러` : 웹 페이지를 크롤링한 다음 색인을 생성하는 자동 소프트웨어로 구글에서 사용하는 Gogglebot이 대표적인 크롤러입니다.

`랭킹(순위 지정)` : 사용자가 검색어를 입력하면 여러 요소를 바탕으로 색인에서 관련성 있는 답변을 찾기 위해 품질 높은 콘텐츠를 결정하고 최상의 사용자 환경과 적절한 답변을 제공할 수 있는 여러 요소를 고려해서 순위를 결정합니다.

**게재 및 순위 개선하는 방법**

- 모바일 친화적인 페이지를 만들어 빠르게 로드할 수 있는 환경 만들기
- 사용자가 윈하는 품질 높은 콘텐츠를 포함하고 최신 상태로 유지
- 사용하는 브라우저 환경에 알맞은 가이드 라인 지키기

<br/>

## 검색 엔진 최적화(SEO)란?

검색 엔진 최적화(search engine optimization)은 웹 페이지 검색엔진이 자료를 수집하고 순위를 매기는 방식에 맞게 웹 페이즈를 구성해서 검색 결과의 상위에 나올 수 있도록 하는 작업입니다.

2000년대 초반의 SEO의 시책에서는 HTML 태깅 개선을 통한 랭크 상승 방식을 지향했지만 최근의 SEO는 검색자의 니즈에 맞추는 것을 중시하고 있습니다. 이런 현상은 다른 웹 검색엔진들 보다 압도적인 시장 점유율을 차지하는 구글의 영향을 통해 검색자의 의도를 이해하고 적절한 콘텐츠를 제작하고, 페이지가 검색 결과 페이지에 잘 노출 되도록 태그와 링크 주소와 같은 방식을 사용하여 개선을 통해 자연 유입 트래픽을 늘리는 방향으로 변화하고 있습니다.

즉, 검색 결과 상위에 페이즈를 노출하기 위해서는 다양한 기법(HTML 태깅 등)을 사용하더라도 콘텐츠 질이 좋지 않다면 사용자의 선택을 받을 수 없으며 콘텐츠의 품질이 높더라도 사용자가 찾지 못하면 사용자 유입이 증가하지 않는 문제가 발생한다. 이런 문제를 해결하기 위해서는 적절한 기법과 품질 높은 콘텐츠의 궁합이 매우 중요해진 시기입니다.

<br/>

## 검색엔진 최적화를 위한 제안 사항

### robots.txt을 사용하여 크롤링 안되는 페이지 알리기

`robots.txt파일`은 검색엔진이 사이트의 일부 URL에 엑세스하여 크롤링할 수 있는지 알려주는 역할입니다. robots.txt 파일이 없으면 검색엔진이 웹 페이지에서 찾을 수 있는 모든 route를 크롤링하며, 불필요한(민감한 정보) route 접근을 막을때 유용합니다.

```
# googlebot management
User-agent: Googlebot
Disallow: /book
Allow: /catagory/
```

robots.txt 파일을 위와 같이 설정하면, 규칙에 따라 googlebot은 /book 페이지를 크롤링하지 않으며, /catagory/로 지각하는 모든 페이즈를 크롤링합니다. [robots.txt 사용법을 참고하세요.](https://developers.google.com/search/reference/robots_txt?hl=ko)

### 사이트맵(sitemap.xml)을 사용하여 내 콘텐츠를 찾을 수 있게 돕기

`sitemap.xml`은 사이트에 있는 페이지, 동영상 및 기타 파일에 관련된 정보를 제공하는 파일입니다. 사이트에서 중요하다고 생각하는 페이지와 파일을 sitemap.xml 파일에 알리면 중요한 관련 정보를 제공하며 누락될 수 있는 페이지를 사전에 방지할 수 있습니다.

사이트맵은 웹사이트 검색엔진최적화 점수를 높이는데 직접적인 영향을 주지 않지만 검색엔진이 크롤리 과정에서 누락될 수 있는 웹페이지에 대한 정보를 제공하여 검색엔진최적화에 긍정적인 영향을 줍니다.

**사이트맵 만드는 과정**

1. 사이트맵 제작 웹사이트, 크롤링 프로그램, Yoast SEO 플러그인을 사용하여 sitemap.xml 파일을 생성합니다.
2. 사이트맵을 구글 서치콘솔 또는 네이버 서치 어드바이저에 제출합니다.

### 고유하고 정확한 페이지 제목을 위해 title 태그 작성하기

`<title>`태그는 사용자와 검색 엔진에게 페이지의 주제를 알려주는 중요한 역할을 합니다. `<title>`태그는 HTML 문서의 `<head>` 태그 안에 위치해야 하며 페이지별로 고유한 제목을 가지고 있어야 합니다.

`<title>` 태그 작성시 유의 사항

- 페이지 내용과 관련 없는 제목을 사용하지 않고 정확한 설명을 작성해야 합니다. 예: 제목없음, 새 페이지1 과 같이 불분명한 내용을 사용하면 안됩니다.
- 사이트의 모든 페이지 또는 여러 페이지에 단일 제목을 사용하지 않으며, 각 페이지마다 고유한 제목을 작성해야 합니다.
- 사용자에게 도움이 되지 않는 매우 긴 제목을 사용하지 않고 간단하지만 설명이 담긴 제목을 사용해야 합니다.

```html
{/* koreanFood.html */}

<html>
  <head>
    <title>2021년 한식 랭킹</title>
    // 페이지별 고유한 제목을 지정합니다.
    <meta name="description" content="올해 가장 맛있는 한식을 뽑는 자리입니다." />
  </head>
</html>
<body>
  ...
</body>
```

```html
{/* japaneseFood.html */}

<html>
  <head>
    <title>2021년 일식 랭킹</title>
    // 페이지별 고유한 제목을 지정합니다.
    <meta name="description" content="올해 가장 맛있는 일식을 뽑는 자리입니다." />
  </head>
</html>
<body>
  ...
</body>
```

### description 메타 태그 사용하기

`description` 메타 태그는 `title` 태그의 제목에 대한 정보를 간략하게 설명할 수 있으며 `<head>` 요소에 작성합니다.

```html
{/* index.html */}

<html>
  <head>
    <title>2021년 양식 랭킹</title>
    <meta
      name="description"
      content="올해 가장 맛있는 일식을 뽑는 자리입니다. 필요한 재료에 대한 정보를 유익하게 알려주는 레시피를 알아가세요!"
    />
  </head>
</html>
<body>
  ...
</body>
```

`description` 메타 태그는 페이지의 스니펫으로 사용할 수 있으며 Google에서 페이지에 표시되는 텍스트 중에 사용자의 검색어와 잘 어울리는 텍스트가 있는 경우 이를 선택할 수도 있습니다.

적절한 description 작성하는 방법

- 페이지 콘텐츠를 정확하게 요약해야합니다.
- 각 페이지마다 고유한 설명을 사용해야 합니다. (모든 페이지가 동일한 설명을 사용하지 않아야 합니다.)
- 설명을 키워드로만 채우는 경우 및 `description` 메타 태그에 문서 전체 내용을 복사하여 붙여넣지 말아야 합니다.

### 제목 태그(h1, h2 ...)를 사용하여 중요한 텍스트 강조하기

의미 있는 제목을 사용하여 중요한 주제를 표시하고 콘텐츠의 계층 구조를 만들어 사용자가 쉽게 문서를 탐색할 수 있도록 해야합니다.

- 페이지에서 꼭 필요한 부분만 제목을 사용해야합니다. (제목을 너무 남용하면 주제의 파악이 힘들어집니다.)
- 제목을 너무 길게 작성하지 말고 스타일을 위해서 제목 구조를 나타내면 안됩니다.

```html
{/* index.html */}

<html>
  <head>
    <title>2021년 양식 랭킹</title>
    <meta
      name="description"
      content="올해 가장 맛있는 일식을 뽑는 자리입니다. 필요한 재료에 대한 정보를 유익하게 알려주는 레시피를 알아가세요!"
    />
  </head>
</html>
<body>
  <h1>양식 레시피</h1>
  ...

  <h2>토마토 파스타 레시피</h2>
  ... ...
  <h2>알리오 올리오파스타 레시피</h2>
</body>
```

<br/>

> 위에서 설명한 '고유하고 정확한 페이지 제목을 위해 title 태그 작성하기', 'description 메타 태그 사용하기', '제목 태그(h1, h2 ...)를 사용하여 중요한 텍스트 강조하기' 와 같은 SEO를 통틀어서 시멘틱 요소(Semantic Elements)를 적절하게 사용하는 방식을 세분한 내용입니다.
>
> 시멘틱 요소를 적절하게 사용하기 위해서는 위의 3가지 방법 외에도 의미를 가지지 않는 `<div>`, `<span>`으로만 HTML을 구성하지 않고 의미 있는 태그를 적절하게 사용하는 방식을 고수해야 합니다.

```html
{/* 시멘틱 요소를 적절하게 사용하지 않은 예시 */}

<div>
  <div>
    <div>포스팅 제목</div>
    <div>포스팅 관련 내용</div>
  </div>
  <div>First Group</div>
  <span>First Group에 관련된 내용이 들어갑니다.</span>
</div>
```

```html
{/* 시멘틱 요소를 적절하게 사용한 예시 */}

<body>
  <header>
    <h1>포스팅 제목</h1>
    <p>포스팅 관련 내용</p>
  </header>
  <main>
  	<h2>First Group</h2>
  	<p>First Group에 관련된 내용이 들어갑니다.</p>
  <main>
</body>
```

<br/>

### 구조화된 데이터 마크업 추가하기

구조화된 데이터란 사이트 페이지에 추가할 수 있는 코드로, 검색엔진에 콘텐츠를 설명해주기 때문에 검색엔진이 페이지에 어떤 내용이 있는지 더 잘 이해할 수 있습니다.

구조화된 데이터 마크업을 사용하면 결과를 표시하는 것 외에도, 관련성 있는 검색 결과를 추가 설명하는 방법을 제공합니다. 예를 들어 식당을 검색하게 되면 가격, 평점, 카테고리 정보 등이 노출된 부분을 볼 수 있는 페이즈를 볼 수 있습니다.

<img src="https://user-images.githubusercontent.com/57402711/126251489-8b354512-ebc2-47f2-bdd5-240a8e8963e7.png" alt="image" style="zoom:40%;" />

**구조회된 데이터를 작성하는 방법**

구조회된 데이터 작업을 위해서는 schema.org에서 제공하는 타입(type)과 속성(property)값을 이용하여 제작할 수 있습니다. 구조화된 데이터 작업 시 Microdata와 RDFa, JSON-LD의 세 가지 언어 형식을 지원합니다

```
schema.org는 2011년 6월 2일 google, bing, yahoo 등 당시 검색 엔진 시장에서 점유율이 높은 운영자들이 모여 웹 페이지에서 구조화된 데이터 마크업을 웨한 공통 스키마 작성을 위한 schema.org를 발표했습니다.
```

- JSON_LD(권장): 페이지 헤드 또는 분문의 `<script>` 태그 내에 삽입되는 자바스크립트 표기입니다.

```html
// JSON-LD 형식
<script type="application/ld+json">
  {
    "@context": "http://schema.org",
    "@type": "Person",
    "name": "My Site Name",
    "url": "http://www.mysite.com",
    "sameAs": [
      "https://www.facebook.com/myfacebook",
      "http://blog.naver.com/myblog",
      "http://storefarm.naver.com/mystore"
    ]
  }
</script>
```

- 마이크로데이터: HTML 콘텐츠 내에 구조화된 데이터를 사용되는 개방형 커뮤니티 HTML 사양입니다. RDFa와 같이 HTML 태그 속성을 사용해 구조화된 데이터를 표시하려는 속성의 이름을 지정합니다. 대개 페이지 본문에 사용되지만 헤드에 사용될 수 있습니다.

```html
<span itemscope="" itemtype="http://schema.org/Organization">
  <link itemprop="url" href="http://www.mysite.com" />
  <a itemprop="sameAs" href="https://www.facebook.com/myfacebook"></a>
  <a itemprop="sameAs" href="http://blog.naver.com/myblog"></a>
  <a itemprop="sameAs" href="http://storefarm.naver.com/mystore"></a>
</span>
```

- RDFa: 사용자에게 표시되며 검색엔진에 제시하려는 콘텐츠에 해당하는 HTML 태그 속성을 도입하여 연결된 데이터를 지원하는 HTML 확장입니다.

```html
<p vocab="http://schema.org/">
  My name is Manu Sporny and you can give me a ring via 1-800-555-0199.
</p>
```

<br/>

### 단순한 URL은 콘텐츠 정보를 전달하기

웹사이트 문서와 관련된 설명을 제공하는 카테고리 및 파일 이름을 만들면 사이트를 더 잘 구성하는 데 도움이 될 뿐만 아니라 콘텐츠에 관심을 두고 있는 사용자가 좀 더 쉽게 사용할 수 있으며 이들에게 더욱 친숙한 URL을 만들 수 있습니다. 인식할 수 있는 단어가 거의 없는 긴 암호문과도 같은 URL에 겁을 먹는 사용자도 있을 수 있습니다.

- **적절한 URL을 사용하기**

  ```
  // 사용자에게 혼란을 줄 수 있는 URL
  https://github.com/park-moen/folder1/22447478/x2/14032015.html

  // URL을 보고 바로 문맥을 이해할 수 있는 URL
  https://github.com/park-moen/VanillaJS_fn/issues/new
  ```

- **canonical URL을 사용하여 대표 URL 지정하기**

> - http://github.com/park-moen/aaa
> - https://github.com/park-moen/aaa
> - https://github.com/park-moen/b.html
> - https://github.com/park-moen/b/word

같은 페이지가 위처럼 여러 가지 URL을 가질 때, 검색엔진은 그중 하나를 표준으로 지정한다. 그리고 나머지는 표준 URL의 복사본으로 간주합니다. 하지만 내가 생각하는 표준 URL이 지정될지 안될지는 알 수 없기 때문에 canonical URL을 사용하여 표준 URL을 직접 지정해주는 것이 바람직합니다.

canonical URL을 지정하기 위해서는 HTML Header 영역에 canonical URL을 넣으면 표준 URL을 지정할 수 있습니다.

```html
<link rel="canonical" href="https://example.com/" />
```

<br/>

## 참고자료

- [구글 SEO 초보자 가이드](https://support.google.com/webmasters/answer/7451184?hl=ko)
- [검색엔진 최적화를 위한 9가지 방법](https://blueshw.github.io/2020/06/14/seo/)
- [검색엔진최적화는 무엇인가?](https://www.ascentkorea.com/what-is-seo/)
