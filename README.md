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
    <div id="app">
        hihihi
    </div>
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



`.babelrc` 생성

```

```



어디까지 캐싱할 건지, 어디까지 오프라인 지원할 건지

Medium 조은 pwa









# Azure!

@justinchronicle 

https://github.com/devkimchi/pwa-workshop/blob/master/docs/step-01.md



CI/CD 도구 -> Jenkins, gitlab ..





