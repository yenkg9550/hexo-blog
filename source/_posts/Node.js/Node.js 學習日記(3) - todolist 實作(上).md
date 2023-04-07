---
title: Node.js 學習日記(3) - todolist 實作(上)
date: 2023-01-18 22:15:07
tags: Node.js
categories: Node.js
---
Node.js 學習日記(3) - todolist 實作
<!--more-->
![](/images/Node02.png)
### 起手式
1. 先建立 package.json 的環境 
```
npm init 
```
2. 載入 createServer 模組
```js
const http = require('http'); // 1. 先載入 http
 
const request = (req, res) => { // 3. 請求 / 回傳
  res.writeHead(200, {"Conent-Type":"text/plain"}); 
  res.write("Hello");
  res.end()
}

const server = http.createServer(request); // 2.使用 createServer 並建立 port 號
server.listen(3005);
```

**********

### 建立 server，新增首頁 and 404 頁面
```js
const http = require('http'); 
 
const request = (req, res) => { 
  const headers = { // 1. 這裡宣告 headers
    "Conent-Type":"text/plain"
  }
  if (req.url == "/" && req.method == "GET") { // 2. 加入 if 判斷路徑跟 methods
    res.writeHead(200, headers); 
    res.write("index");
    res.end()
  } else if (req.url == "/" && req.method == "DELETE") {
    res.writeHead(200, headers); 
    res.write("成功刪除");
    res.end()
  } else { 
    res.writeHead(404, headers); 
    res.write("not found 404");
    res.end()
  }
}

const server = http.createServer(request);
server.listen(3005);
```

**********

### 調整 headers 資訊、設置回傳 JSON 與 cors 資訊

```js
const http = require('http'); 
 
const request = (req, res) => { 
  const headers = { // 設置表頭資訊
    "Access-Control-Allow-Headers": 'Conent-Type, Authorization, Content-Length, X-Requested-With',
    "Access-Control-Allow-Origin": '*',
    "Access-Control-Allow-Methods": 'PATCH, POST, GET, OPTIONS, DELETE',
    "Conent-Type": 'application/json'
  }
  if (req.url == "/" && req.method == "GET") {
    res.writeHead(200, headers); 
    res.write(JSON.stringify({
      "status": "success",
      "data": [],
    }));
    res.end()
  } else { 
    res.writeHead(404, headers); 
    res.write(JSON.stringify({
      "status": "false",
      "message": "無此路徑" ,
    }));
    res.end()
  }
}

const server = http.createServer(request);
server.listen(3005);
```

**********

### 設定 options API

```js
const http = require('http'); 
 
const request = (req, res) => { 
  const headers = {
    "Access-Control-Allow-Headers": 'Conent-Type, Authorization, Content-Length, X-Requested-With',
    "Access-Control-Allow-Origin": '*',
    "Access-Control-Allow-Methods": 'PATCH, POST, GET, OPTIONS, DELETE',
    "Conent-Type": 'application/json'
  }
  if (req.url == "/" && req.method == "GET") {
    res.writeHead(200, headers); 
    res.write(JSON.stringify({
      "status": "success",
      "data": [],
    }));
    res.end()
  } else if (req.method == "OPTIONS") {  // 在這裡加上 OPTIONS
    res.writeHead(200, headers);
    res.end()
  } else { 
    res.writeHead(404, headers); 
    res.write(JSON.stringify({
      "status": "false",
      "message": '無此路徑' ,
    }));
    res.end()
  }
}

const server = http.createServer(request);
server.listen(3005);
```

**********

### 取得所有待辦、增加 UUID NPM

```js
const http = require('http');
const { v4: uuidv4 } = require('uuid'); // 1. 載入 uuid 模組

// 2. 宣告一個 todos
const todos = [
  {
    "title": "今天要刷牙",
    "id": uuidv4()
  }
];

const request = (req, res) => { 
  const headers = {
    "Access-Control-Allow-Headers": 'Conent-Type, Authorization, Content-Length, X-Requested-With',
    "Access-Control-Allow-Origin": '*',
    "Access-Control-Allow-Methods": 'PATCH, POST, GET, OPTIONS, DELETE',
    "Conent-Type": 'application/json'
  }
  if (req.url == "/todos" && req.method == "GET") { // 3. 修改路徑為 todos
    res.writeHead(200, headers); 
    res.write(JSON.stringify({
      "status": "success",
      "data": todos, // 4. 改為 todos
    }));
    res.end()
  } else if (req.method == "OPTIONS") {
    res.writeHead(200, headers);
    res.end()
  } else { 
    res.writeHead(404, headers); 
    res.write(JSON.stringify({
      "status": "false",
      "message": '無此路徑' ,
    }));
    res.end()
  }
}

const server = http.createServer(request);
server.listen(3005);
```

**********

### POST API router 環境建立

```js
const http = require('http');
const { v4: uuidv4 } = require('uuid');

const todos = [
  {
    "title": "今天要刷牙",
    "id": uuidv4()
  }
];

const request = (req, res) => { 
  const headers = {
    "Access-Control-Allow-Headers": 'Conent-Type, Authorization, Content-Length, X-Requested-With',
    "Access-Control-Allow-Origin": '*',
    "Access-Control-Allow-Methods": 'PATCH, POST, GET, OPTIONS, DELETE',
    "Conent-Type": 'application/json'
  }
  if (req.url == "/todos" && req.method == "GET") {
    res.writeHead(200, headers); 
    res.write(JSON.stringify({
      "status": "success",
      "data": todos, //
    }));
    res.end()
  } else if (req.method == "OPTIONS") {
    res.writeHead(200, headers);
    res.end()
  } else if (req.url == "/todos" && req.method == "POST") { // 在這裡新增 POST
    res.writeHead(200, headers); 
    res.write(JSON.stringify({
      "status": "success",
      "data": 1234, //
    }));
    res.end()
  } else { 
    res.writeHead(404, headers); 
    res.write(JSON.stringify({
      "status": "false",
      "message": '無此路徑' ,
    }));
    res.end()
  }
}

const server = http.createServer(request);
server.listen(3005);
```