# Webpack 사용하기

<br />

## Webpack 개념 정리

현재 자바스크립트는 광범위하게 사용되고 있는 반면에 파일별로 관리할 수 있는 모듈을 지원하지 않았습니다.(ES6 이후에 지원을 하지만 현재 지원하지 않는 브라우저도 아직 존재한다.) 그렇다고 여러 개의 파일을 브라우저에 로딩하는 것은 그만큼 네트워크에 무리를 주는 행동이였습니다. 또한 자바스크립트는 파일별로 분리하여 식별자 이름을 작성해도 파일별 스코프를 가지고 있지 않고 공통의 스코프를 가지고 있어서 식별자 이름 충돌이 발생하는 문제가 존재했습니다.

이러한 문제를 해결하기 위해서 즉시실행함수(IIFE)를 사용해서 모듈 기능을 대처하려는 노력을 하였지만 하나의 파일에서 모든 작업을 해야하는 또 다른 문제가 존재했습니다. 이러한 문제를 해결하기 위해서 모듈 번들러 관리 라이브러리가 등장하게 되었고 현재 가장 많이 사용하고 있는 모듈 번들러로는 Webpack입니다.

<img src="https://user-images.githubusercontent.com/57402711/124529586-e02bab00-de45-11eb-85a3-2c365bcb45ed.png" alt="image" style="zoom:30%;" />

Webpack에서 사용하는 네 가지 개념에 대해서 정리 후 예시 및 설정 방법에 대해서 알아 보겠습니다.

### 엔트리

*엔트리 포인트*는 webpack이 내부의 디펜던시 그래프를 생성하기 위해 사용해야 하는 모듈이며, webpack은 엔트리 포인트가 의존하는 다른 모듈과 라이브러리를 찾습니다.

```
Tip

디펜던시 그래프는 webpack 공식문서에서 사용하는 개념으로 의존하고 있는 모든 모듈에 대해 재귀적으로 빌드한 다음 설정한 엔트리 모듈로 번들합니다.(여러가지 파일을 재귀적으로 찾아서 엔트리 파일로 통일한다고 생각하면 이해하기 쉽습니다.)
```

위의 설명을 조금 더 직관적으로 설명하면 webpack은 엔트리 모듈을 통해서 모든 파일을 재귀적으로 찾아서 하나의 파일로 묶습니다.

간단한 설정을 해보겠습니다.

```js
// webpack.config.js

module.exports = {
  entry: {
    main: './src/index.js',
  },
};

// html파일에서 사용할 JS의 시작점은 src.index.js입니다.
```

### 아웃풋

output 속성은 생성된 번들을 내보낼 위치와 파일 이름을 지정하는 방법을 webpack에게 알려주는 역할입니다. 가장 보편적으로 사용하는 폴더 이름을 `./dist`입니다.

```js
// webpack.config.js

const path = require('path'); // node.js에서 지원하는 path 모듈을 사용할 수 있습니다.

module.exports = {
  output: {
    path: path.resolve(__dirname, 'dist'),
    filename: 'bundle.js',
  },
};
```

화면을 보여줄 html 파일에 번들한 결과인 bundle.js를 script tag에 삽입합니다.

```html
<!-- index.html -->

<body>
  <script src="./dist/bundle.js"></script>
</body>
```

webpack은 커맨드 라인을 빌드가 가능하며 ``webpack.config.js` 파일을 만들면 간단한게 `webpack` 을 커맨들 라인에 작성하면 됩니다.

```js
// webpack.config.js 파일을 작성하지 않은 경우
$ webpack ./src/index.js ./dist/bundle.js

// webpack.config.js 파일을 작성한 경우
$ webpack
```

### 로더

webpack은 모든 파일은 모듈로 관리합니다.(js, json 파일뿐만 아니라 이미지, 폰트, 스타일시드도 모듈로 관리하는게 목적) 그러나 webpack은 js 파일만 이해합니다. 로더를 사용하면 webpack에서 다른 유형의 파일을 사용할 수 있습니다.

로더는 `test` 와 `use` 키로 구성된 객체로 설정합니다.

- 변환이 필요한 파일(들)을 식별하는 `test` 속성
- 변환을 수행하는데 사용하는 로더를 가리키는 `use` 속성

**css-loader, style-loader**

자주 사용하는 로더인 css-loader 와 style-loader로 예시를 들게습니다.

```
$ npm i --save-dev css-loader style-loader
```

```js
module: {
  rules: [
    {
      test: /\.css$/,  // 정규 표현식을 사용하여 확장자 css가 보이는 모든 파일을 로더한다.
      use: ['style-loader' ,'css-loader'],
    },
  ],
},
```

- `css-loader` : css 파일을 자바스크립트로 변환하는 로더
- `style-loader` : css-loader를 통해 자바스크립트로 변환된 스타일시트를 동적으로 돔에 추가하는 로더

위 설정에서는 `test` 와 `use` 라는 두 가지 필수 속성을 가진 하나의 모듈을 위해 `rules` 속성을 정의했습니다. webpack의 컴파일러가 이해하는 방식을 예로 들어 보겠습니다.

> "webpack 컴파일러야 `require()` / `import` 문 내에서 '.css' 파일로 확인되는 경로를 발경하면 번들러에 추가하기 전에 `style-loader`, `css-loader` 를 사용하여 변환해 주렴"

**handlebars-loader**

js 파일, css 파일처럼 html 파일 또한 js 코드에 따라서 랜더링 시키고 싶은 경우가 있습니다. 이런 경우 템플릿 엔진인 handlebars를 사용할 수 있습니다. (다른 템플릿 엔진을 사용해도 괜찮습니다.)

```
$ npm i --save-dev handlebars-loader handlebars
```

handlebars를 install하지 않고 loader만 install 하는 경우 경고가 발생하기 때문에 handlebars도 npm에서 같이 다운 받아야 합니다.

```js
module: {
  rules: [
    {
      test: /\.hbs$/,  // 확장자 이름은 hbs를 사용하면 파일을 만들때 간편합니다.(취향 존중)
      use: ['handlebars-loader'],
    },
  ],
},
```

```html
<!-- template.hbs -->

<main>
  <ul>
    <li>사과</li>
    <li>바나나</li>
    <li>딸기</li>
  </ul>
</main>
```

```js
// index.js

const tempHtml = require('./pages/template.hbs');

console.log(tempHtml());
```

### 플로그인

로더는 파일 단위로 특정 유형의 모듈을 변환하는 데 사용되자만, 플러그인은 번들된 결과를 처리합니다. 번들된 결과물을 최적화, 환경 변수 주입 등과 같은 광범위한 작업을 수행합니다.

플러그인을 사용하려면 `require ()`를 통해 플러그인을 요청하고 `plugins` 배열에 추가해야 합니다. 대부분의 플러그인은 옵션을 통해 사용자가 지정할 수 있습니다. 다른 목적으로 플러그인을 여러 번 사용하도록 설정할 수 있으므로 `new` 연산자로 호출하여 플러그인의 인스턴스를 만들어야 합니다.

**HtmlWebpackPlugin**

HtmlWebpackPlugin은 html 파일을 후처리하는데 사용합니다. HtmlWebpackPlugin 플러그인을 사용하여 빌드하면 html 파일로 아웃풋에 생성됩니다.

**CleanWebpackPlugin**

CleanWebpackPlugin은 빌드 이전에 결과물을 제거하는 플러그인입니다. 과거 파일이 남아 있을 수 있는 문제를 해결합니다.

**MiniCssExtractPlugin**

스타일시트가 점점 많아지면 하나의 자바스크립트 결과물로 만드는 것이 부담일 수 있습니다. 번들 결과에서 스트일시트 코드만 뽑아서 별도의 CSS 파일로 만들어 역할에 따라 파일을 분리하는 것이 좋습니다..

```
$ npm install -D html-webpack-plugin clean-webpack-plugin
```

```js
const HtmlWebpackPlugin = require('html-webpack-plugin');
const { CleanWebpackPlugin } = require('clean-webpack-plugin');
const MiniCssExtractPlugin = require('mini-css-extract-plugin');


module.exports = {
  plugins: [
    new HtmlWebpackPlugin({
      filename: 'index.html', // output file name
      template: 'index.html', // template file name
      collapseWhitespace: true, // 빈칸 제거
    	removeComments: true, // 주석 제거
    }),
    new MiniCssExtractPlugin({ filename: '[name].css' }),
    new CleanWebpackPlugin({
      cleanAfterEveryBuildPatterns: ['dist'],
    }),
  ],
```

### Mode

`mode` 파라미터를 `development`, `production` 또는 `none`으로 설정하면 webpack에 내장된 환경별 최적화를 활성화 할 수 있습니다. 기본값은 `production` 입니다. 환경 변수를 사용하여 production 모드와 development 모드를 변경하는 방식을 사용 할 수 있습니다.

```js
module.exports = {
  mode: 'production',
};
```
