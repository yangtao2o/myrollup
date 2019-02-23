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