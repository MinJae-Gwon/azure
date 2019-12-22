## PWA?

웹앱을 구현하기 위한 모든 기술들의 집합 -> 특정 기술만을 지칭하는 것이 아님



F.I.R.E

* Fast
* Intergrated
* reliable
* engaging



1. Service worker

백그라운드에서 동작하는 Worker -> 캐시를 불러와 Offline 지원 and 서버 불안정시 사용성 증가

2. Web App Manifest

웹앱의 config를 담당 -> add to home screen





## Go!

https://github.com/techhtml/2019-12-21-pwa-workshop

개발자 도구 -> application -> manifest에서 정보 확인 가능



PWA dir 생성! 여기에 웹앱을 구현 해보자

1. pakage.json

```bash
npm init
```



Enter 입력을 계속 하면 기본 설정으로 pakage.json 생성 가능!

```bash
(base) minjaeui-MacBookPro:pwa_final minjae$ npm init
This utility will walk you through creating a package.json file.
It only covers the most common items, and tries to guess sensible defaults.

See `npm help json` for definitive documentation on these fields
and exactly what they do.

Use `npm install <pkg>` afterwards to install a package and
save it as a dependency in the package.json file.

Press ^C at any time to quit.
package name: (pwa_final) 
version: (1.0.0) 
description: 
entry point: (index.js) 
test command: 
git repository: 
keywords: 
author: 
license: (ISC) 
About to write to /Users/minjae/pwa_final/package.json:

{
  "name": "pwa_final",
  "version": "1.0.0",
  "description": "",
  "main": "index.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1"
  },
  "author": "",
  "license": "ISC"
}


Is this OK? (yes) 
```





2. dir 최상위 위치에 폴더 생성

   public

   src 





3. pakage install

npm install --save react react-dom

npm install --save-dev webpack webpack-cli webpack-dev-server html-webpack-plugin

npm install --save-dev @babel/core @babel/preset-env @babel/preset-react babel-loader



`babel-loader`는 `webpack`이 .js 파일들에 대해 babel을 실행하도록 만들어주고, `babel-core`는 babel이 실제 동작하는 코드이고, `babel-preset-env`는 babel이 동작할 때 지원범위가 어느정도까지 되어야 하는지에 대해 지정하도록 만들어주는 패키지

이렇게 설치를 진행하고 나면 Babel과 Webpack을 사용할 준비 완료



3-1.

`package.json`파일은  npm와 결합해 특정 명령어를 실행하거나 npm 프로젝트의 환경을 담는 파일

```bash
npm run 명령어이름
```



```json
//pakage.json
{
  ...
  "scripts": {
    "start": "webpack-dev-server --mode development --open --hot",
    "build": "webpack --mode production"
  },
  ...
}
```



4. file 생성

Src 폴더 내부에 `index.html` `index.js`  파일 생성

```html
// index.html
<!DOCTYPE html>
<html lang="ko">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>리액트 메모앱</title>
</head>
<body>
    <div id="app"></div>
    <script src='index.bundle.js'></script>
</body>
</html>
```



```js
// index.js
import React from 'react';
import ReactDOM from 'react-dom';


const App = () => (
  <div className="app">
    Hi
  </div>
)

ReactDOM.render(<App />, document.getElementById('app'));

```



`.babelrc` 파일생성

Babel은 최신 자바스크립트를 `ES5`버전에서도 돌아갈 수 있도록 변환(Transpiling)

```
{
    "presets": [
      "@babel/env",
      "@babel/react"
    ]
}
```



`webpack.config.js` 파일 생성

`webpack.config.js` 파일은 앞서 설치해준 `webpack`을 실행 시 어떤 옵션을 사용할지 지정해주는 js파일

웹팩의 개념 => https://www.daleseo.com/webpack-basics/

```js
const path = require('path');
const HtmlWebpackPlugin = require('html-webpack-plugin');

module.exports = {
    //웹팩은 기본적으로 여러 개의 자바스크립트 모듈을 하나의 파일로 묶어내는 번들러입니다. 따라서 웹팩은 다른 모듈을 사용하고 있는 최상위 자바스크립트 파일이 어디에 있는지 알아야 하며, 설정 파일에서 이를 Entry 속성으로 명시
    entry: './src/index.js',
   //__dirname은 NodeJS에서 현재 프로젝트 디렉토리를 의미
    output: {
      path: path.join(__dirname, '/dist'),
      filename: 'index.bundle.js'
    },
   //test 항목에 정의된 정규식에 매칭되는 파일은 use 항목에 등록된 로더를 통해서 처리되게 됩니다. 
    module: {
      rules: [
        {
          test: /\.js$/,
          exclude: /node_modules/,
          use: {
            loader: 'babel-loader'
          }
        }
      ]
    },
    plugins: [
      new HtmlWebpackPlugin({
        template: './src/index.html'
      }),  
    ]
  }
```

위 파일은 `entry`에 현재 위치의 `index.js`파일을 들어가 모든 `import`를 찾아오고, `module -> rules -> include`에 있는 `.js`로 된 모든 파일을 babel로 처리해줍니다.(`exclue`에 있는 부분인 `node_modules`폴더는 제외합니다.)





그 다음, 터미널에서 `npx webpack` 커맨드를 실행하면 웹팩이 `index.js` 파일을 읽어들여 `dist` 디렉토리에 `index.bundle.js` 파일로 묶어냅니다.

```bash
npx webpack
```





5. 번들링된 js파일을 보여줄 HTML파일

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
</head>
<body>
    <div id="app">

    </div>
  	<!-- 추가! -->
    <script type="text/javascript" src="/dist/bundle.js"></script>
</body>
</html>
```



## manifest



```bash
npm install webpack-pwa-manifest
```



```js
const path = require('path');
const HtmlWebpackPlugin = require('html-webpack-plugin');
const WebpackPwaManifest = require('webpack-pwa-manifest');

module.exports = {
  entry: './src/index.js',
  output: {
    path: path.join(__dirname, '/dist'),
    filename: 'index.bundle.js'
  },
  module: {
    rules: [
      {
        test: /\.js$/,
        exclude: /node_modules/,
        use: {
          loader: 'babel-loader'
        }
      }
    ]
  },
  plugins: [
    new HtmlWebpackPlugin({
      template: './src/index.html'
    }),
    new WebpackPwaManifest({
        name: "리액트 데모",
        short_name: "데모",
        description: '리액트로 제작한 데모입니다',
        background_color: '#ffffff',
        crossorigin: 'use-credentials',
        theme_color: '#eeeeee',
      //src/assets 폴더 생성후 Icon.png 넣기
        icons: [
            {
                src: path.resolve('src/assets/Icon.png'),
                sizes: [96, 128, 192, 256, 384, 512]
            }
        ]
    })
  ]
}
```





## ServiceWorker

서비스 워커란 웹 브라우저 밖에서 살고 있다.

서비스 워커는 브라우저의 캐시를 뒤져서 클라이언트에게 제공하는 역할을 한다.



네트워크가 오프라인일 때 , 혹은 웹 브라우저를 종료했을 때 캐시 데이터가 있다면, 

인터넷을 통해(네트워크를 통해) 데이터를 가져와 보여주는 것이 아니라 

서비스 워커가 캐시의 데이터를 찾아 그에 맞는 데이터를 클라이언트에게 보여준다. 



```bash
npm install workbox-webpack-plugin
```



```js
const path = require('path');
const HtmlWebpackPlugin = require('html-webpack-plugin');
const WebpackPwaManifest = require('webpack-pwa-manifest');
const { GenerateSW } = require('workbox-webpack-plugin');

module.exports = {
  entry: './src/index.js',
  output: {
    path: path.join(__dirname, '/dist'),
    filename: 'index.bundle.js'
  },
  module: {
    rules: [
      {
        test: /\.js$/,
        exclude: /node_modules/,
        use: {
          loader: 'babel-loader'
        }
      }
    ]
  },
  plugins: [
    new HtmlWebpackPlugin({
      template: './src/index.html'
    }),
    new WebpackPwaManifest({
        name: "리액트 데모",
        short_name: "데모",
        description: '리액트로 제작한 데모입니다',
        background_color: '#ffffff',
        crossorigin: 'use-credentials',
        theme_color: '#eeeeee',
        icons: [
            {
                src: path.resolve('src/assets/Icon.png'),
                sizes: [96, 128, 192, 256, 384, 512]
            }
        ]
    }),
    new GenerateSW({
        include: [/\.html$/, /\.js$/]
    })
  ]
}
```



```js
//index.js
import React from 'react';
import ReactDOM from 'react-dom';
//import { PrimaryButton } from 'office-ui-fabric-react/lib/Button';

const App = () => (
  <div className="app">
    Hi
  </div>
)

ReactDOM.render(<App />, document.getElementById('app'));

if('serviceWorker' in navigator) {
    window.addEventListener('load', () => {
        navigator.serviceWorker.register('/service-worker.js');
    })
}
```



어디까지 캐싱할 건지, 어디까지 오프라인 지원할 건지 고려 해야함

Reference : Medium 조은 pwa 검색









# Azure!

@justinchronicle 

https://github.com/devkimchi/pwa-workshop/blob/master/docs/step-01.md



CI/CD 도구 -> Jenkins, gitlab ..





