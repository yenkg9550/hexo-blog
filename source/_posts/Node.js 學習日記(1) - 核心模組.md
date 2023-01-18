---
title: Node.js 學習日記(1) - 核心模組
date: 2023-01-15 15:37:57
tags: Node.js
categories: Node.js
---
Node.js 學習日記 - 核心模組
<!--more-->

### require、module exports 模組
程式碼片段：
```js
/* app.js */
var content = require('./data') // require 是用來載入 data.js 的 
console.log(content)

/* data.js */
var data = 123
```

這時會發現 console.log 出來的結果會是一個空物件 {}，而不是 123
原因是必須要使用 module.exports = data 來導出

```js
/* app.js */
var content = require('./data')
console.log(content)

/* data.js */
var data = 123
module.exports = data // 必須加上這一段來 exports 出去
```

這時候就會印出正確的答案 123

***********

### exports 模組
接著是使用 exports 的方式：
```js
/* app.js */
var content = require('./data')
console.log(content)

/* data.js */
exports.data = 123 // 這裡直接導出 exports
```
印出來的結果一樣是 123

另外也可以寫成 function 的形式，例如：
```js
/* app.js */
var content = require('./data')
console.log(content.bark())

/* data.js */
exports.bark = function(){
  return "bark!!"
}

// 印出來為 "bark!!"
```

<font color=#FF0000>另外要特別注意 module exports 與 exports 是不能一起用的唷！！</font>

*******

### createServer 模組
若要使用 createServer 可以參考以下程式碼：
```js
var http = require('http'); // 載入原生 http 模組
http.createServer(function(req, res) { // 建立 server 在此處理 戶端向 http server 發送過來的 req
  res.writeHead(200, {"Content-Type": "text/plain"}); // 若為成功 200，格式為 text/plain
  res.write("Hello !!"); // 寫入 "Hello !!"
  res.end(); // 結束
}).listen(8080); // 進入此網站的監聽 port
```

********

### 載入路徑模組
```js
var path = require('path');

// 抓目錄路徑 -->
console.log(path.dirname('/xx/yy/zz.js'))

// 路徑合併 -->
console.log(path.join(__dirname,'/xx'))

// 抓檔案名稱 --> zz.js
console.log(path.basename('/xx/yy/zz.js'))

// 抓副檔名 --> .js
console.log(path.extname('/xx/yy/zz.js'))

// 分析路徑 --> { root: '/', dir: '/xx/yy', base: 'zz.js', ext: '.js', name: 'zz' }
console.log(path.parse('/xx/yy/zz.js'))
```