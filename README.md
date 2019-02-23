# MyRollup
Rollup是一个 JavaScript 模块打包器，可以将小块代码编译成大块复杂的代码，并且使用心得标准化格式，如ES6.

## 文档
中文文档：[rollup.js](https://www.rollupjs.com/guide/zh)

## 起步
#### npm
```bash
git clone https://github.com/yangtao2o/myrollup.git
cd myrollup
npm init
npm i --save-dev rollup-plugin-json
```
#### Rollup
全局安装 Rollup
```bash
npm i rollup -g
```
创建入口文件 src/main.js
```javascript
// main.js
import { version } from '../package.json';
export default function() {
  console.log('version ' + version)
}
```
创建 `rollup.config.js`，并配置：
```javascript
import json from 'rollup-plugin-json';

export default {
  input: 'src/main.js',
  output: {
    file: 'bundle.js',
    format: 'cjs'
  },
  plugins: [
    json()
  ]
}
```
打开 `package.json` 中配置：
```bash
"scripts": {
  "build": "rollup --config rollup.config.js"
},
```
`npm run build` 执行 Rollup，结果如下：
```javascript
'use strict';

var version = "1.0.0";

function main() {
  console.log('version ' + version);
}

module.exports = main;
```

## 工具集成

#### npm packages

* `rollup-plugin-node-resolve`插件告诉 Rollup 如何查找外部模块
* `rollup-plugin-commonjs`将 CommonJS 转换为 ES2015模块
* 
```bash
myrollup git:(master) npm install --save-dev rollup-plugin-node-resolve
```
添加到 配置 中
```javascript
import resolve from 'rollup-plugin-node-resolve';

export default {
  input: 'src/main.js',
  output: {
    file: 'bundle.js',
    format: 'cjs'
  },
  plugins: [
    resolve()
  ]
}
```

#### Babel

* `npm i -D rollup-plugin-babel`

```bash
npm i -D rollup-plugin-babel 
```
配置 config 文件
```javascipt
import json from 'rollup-plugin-json';
import resolve from 'rollup-plugin-node-resolve';
import babel from 'rollup-plugin-babel';

export default {
  input: 'src/main.js',
  output: {
    file: 'bundle.js',
    format: 'cjs'
  },
  plugins: [
    json(),
    resolve(),
    babel({
      exclude: 'node_modules/**'  //这里除外，只编译源文件
    })
  ]
}
```
然后，创建`src/.babelrc`并配置：
```javascript
{
  "presets": [
    ["latest", {
      "es2015": {
        "modules": false
      }
    }]
  ],
  "plugins": ["external-helpers"]
}
```
* "modules"设置为false，防止 Babel 在 Rollup 处理之前，将模块先转成 CommonJS，从而导致 Rollup 的一些处理失败
* `external-helpers`插件允许 Rollup 在包的顶部只引用一次 `helpers`，而不是每个使用它们的模块都引用一遍（默认）
* `.babelrc`文件放置在 `src` 目录下，而不是根目录，这样可以对不同的任务有不同的 `.babelrc` 配置

下载安装所需要的插件：
```bash
npm i -D babel-preset-latest babel-plugin-external-helpers
```

目前运行 `npm run build`，总是出错：
```bash
src/main.js → bundle.js...
[!] (babel plugin) ReferenceError: Unknown option: .caller. Check out http://babeljs.io/docs/usage/options/ for more information about options.
src/main.js
```