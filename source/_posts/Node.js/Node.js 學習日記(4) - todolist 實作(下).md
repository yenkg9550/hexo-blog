---
title: Node.js 學習日記(4) - todolist 實作(下)
date: 2023-01-19 13:41:23
tags: Node.js
categories: Node.js
---
Node.js 學習日記(4) - todolist 實作(下)
<!--more-->

### 新增 POST API
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

  let body = "";
  req.on('data', chunk => {
    body += chunk
  })

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
  } else if (req.url == "/todos" && req.method == "POST") {
    req.on('end', () => { // 監聽 POST 請求結束後
      const title = JSON.parse(body).title; // 利用 JSON.parse 轉換 body
      const todo = { // 建立 todo
        "title": title,
        "id": uuidv4()
      }
      todos.push(todo) // 將 todo 推入 todos
      res.writeHead(200, headers); 
      res.write(JSON.stringify({
        "status": "success",
        "data": todos, //
      }));
      res.end()
    })
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
如果成功的話，就會出現以兩筆資料：
```json
{
"status":"success",
"data":[
    {"title":"今天要刷牙","id":"70cab700-7cf8-4fd5-9e6b-96ed8b18ff01"},
    {"title":"今天要刷牙2","id":"7003c084-cbdb-4929-8f84-8696dcf32918"}
  ]
}
```

**********

### 新增 POST 異常行為

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

  let body = "";
  req.on('data', chunk => {
    body += chunk
  })

  if (req.url == "/todos" && req.method == "GET") {
    res.writeHead(200, headers); 
    res.write(JSON.stringify({
      "status": "success",
      "data": todos,
    }));
    res.end()
  } else if (req.method == "OPTIONS") {
    res.writeHead(200, headers);
    res.end()
  } else if (req.url == "/todos" && req.method == "POST") {
    req.on('end', () => { 
      try { // 1. 加上 try catch 來回傳成功, 失敗
        const title = JSON.parse(body).title;
        if (title != undefined){ // 2. 加入 if 判斷 title 是否有值
          const todo = {
            "title": title,
            "id": uuidv4()
          }
          todos.push(todo)
          res.writeHead(200, headers); 
          res.write(JSON.stringify({
            "status": "success",
            "data": todos,
          }));
          res.end()
        } else { // 3. 若 title 沒有值的話，則跑下面這段
          res.writeHead(404, headers); 
          res.write(JSON.stringify({
            "status": "false",
            "message": '欄位未填寫正確，或無此 todo id' ,
          }));
          res.end()
        }
      } catch(error) {
        res.writeHead(404, headers); 
        res.write(JSON.stringify({
          "status": "false",
          "message": '欄位未填寫正確，或無此 todo id' ,
        }));
        res.end()
      }
    })
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
接著使用 POST 測試，送出以下資料：
```json
{
  "qq": "123"
}
```
會回傳 else 裡面的內容：
```json
{"status":"false","message":"欄位未填寫正確，或無此 todo id"}
```

**********

### 重構 POST API 異常行為

可以先新增一支 errorHandle.js，並將 error 的程式碼移動到裡面

```js
// errorHandle.js
const headers = { // 這裡也要載入 headers
  "Access-Control-Allow-Headers": 'Conent-Type, Authorization, Content-Length, X-Requested-With',
  "Access-Control-Allow-Origin": '*',
  "Access-Control-Allow-Methods": 'PATCH, POST, GET, OPTIONS, DELETE',
  "Conent-Type": 'application/json'
}

function errorHandle(res) {
  res.writeHead(404, headers); 
  res.write(JSON.stringify({
    "status": "false",
    "message": '欄位未填寫正確，或無此 todo id' ,
  }));
  res.end()
}

module.exports = errorHandle
```

接著回到 server.js 進行載入
```js
// server.js
const http = require('http');
const { v4: uuidv4 } = require('uuid');
const errorHandle = require('./errorHandle'); // 這裡載入 errorHandle

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

  let body = "";
  req.on('data', chunk => {
    body += chunk
  })

  if (req.url == "/todos" && req.method == "GET") {
    res.writeHead(200, headers); 
    res.write(JSON.stringify({
      "status": "success",
      "data": todos,
    }));
    res.end()
  } else if (req.method == "OPTIONS") {
    res.writeHead(200, headers);
    res.end()
  } else if (req.url == "/todos" && req.method == "POST") {
    req.on('end', () => { 
      try {
        const title = JSON.parse(body).title;
        if (title != undefined) {
          const todo = {
            "title": title,
            "id": uuidv4()
          }
          todos.push(todo)
          res.writeHead(200, headers); 
          res.write(JSON.stringify({
            "status": "success",
            "data": todos,
          }));
          res.end();
        } else {
          errorHandle(res); // 替換成 errorHandel
        }
      } catch(error) {
        errorHandle(res); // 替換成 errorHandel
      }
    })
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

### 刪除所有待辦 API

```js
const http = require('http');
const { v4: uuidv4 } = require('uuid');
const errorHandle = require('./errorHandle');
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

  let body = "";
  req.on('data', chunk => {
    body += chunk
  })

  if (req.url == "/todos" && req.method == "GET") {
    res.writeHead(200, headers); 
    res.write(JSON.stringify({
      "status": "success",
      "data": todos,
    }));
    res.end()
  } else if (req.method == "OPTIONS") {
    res.writeHead(200, headers);
    res.end()
  } else if (req.url == "/todos" && req.method == "POST") {
    req.on('end', () => { 
      try {
        const title = JSON.parse(body).title;
        if (title != undefined){
          const todo = {
            "title": title,
            "id": uuidv4()
          }
          todos.push(todo)
          res.writeHead(200, headers); 
          res.write(JSON.stringify({
            "status": "success",
            "data": todos,
          }));
          res.end();
        } else {
          errorHandle(res);
        }
      } catch(error) {
        errorHandle(res);
      }
    })
  } else if(req.url == "/todos" && req.method == "DELETE") {
    todos.length = 0;
    res.writeHead(200, headers); 
    res.write(JSON.stringify({
      "status": "success",
      "data": todos,
      "isDelete": "yes"
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

**********

### 刪除單筆資料 API

```js
const http = require('http');
const { v4: uuidv4 } = require('uuid');
const errorHandle = require('./errorHandle');
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

  let body = "";
  req.on('data', chunk => {
    body += chunk
  })

  if (req.url == "/todos" && req.method == "GET") {
    res.writeHead(200, headers); 
    res.write(JSON.stringify({
      "status": "success",
      "data": todos,
    }));
    res.end()
  } else if (req.method == "OPTIONS") {
    res.writeHead(200, headers);
    res.end()
  } else if (req.url == "/todos" && req.method == "POST") {
    req.on('end', () => { 
      try {
        const title = JSON.parse(body).title;
        if (title != undefined){
          const todo = {
            "title": title,
            "id": uuidv4()
          }
          todos.push(todo)
          res.writeHead(200, headers); 
          res.write(JSON.stringify({
            "status": "success",
            "data": todos,
          }));
          res.end();
        } else {
          errorHandle(res);
        }
      } catch(error) {
        errorHandle(res);
      }
    })
  } else if(req.url == "/todos" && req.method == "DELETE") {
    todos.length = 0;
    res.writeHead(200, headers); 
    res.write(JSON.stringify({
      "status": "success",
      "data": todos,
      "isDelete": "yes"
    }));
    res.end()
  } else if(req.url.startsWith("/todos/") && req.method == "DELETE") { // 加入 startsWith 判斷開頭是否為 /todos
    const id = req.url.split('/').pop(); // 加入 split 刪除 /，使用 pop()方法會移除並回傳陣列的最後一個元素，並改變陣列長度
    const index = todos.findIndex(ele => ele.id == id); // 尋找相符合的 id
    if(index !== -1) { // 若有找到則跑以下內容
      todos.splice(index, 1); // 刪除資料
      res.writeHead(200, headers); 
      res.write(JSON.stringify({
        "status": "success",
        "data": todos,
      }));
      res.end();
    } else {
      errorHandle(res);
    }
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

### 編輯單筆資料 API

```js
const http = require('http');
const { v4: uuidv4 } = require('uuid');
const errorHandle = require('./errorHandle');
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

  let body = "";
  req.on('data', chunk => {
    body += chunk
  })

  if (req.url == "/todos" && req.method == "GET") {
    res.writeHead(200, headers); 
    res.write(JSON.stringify({
      "status": "success",
      "data": todos,
    }));
    res.end()
  } else if (req.method == "OPTIONS") {
    res.writeHead(200, headers);
    res.end()
  } else if (req.url == "/todos" && req.method == "POST") {
    req.on('end', () => { 
      try {
        const title = JSON.parse(body).title;
        if (title != undefined){
          const todo = {
            "title": title,
            "id": uuidv4()
          }
          todos.push(todo)
          res.writeHead(200, headers); 
          res.write(JSON.stringify({
            "status": "success",
            "data": todos,
          }));
          res.end();
        } else {
          errorHandle(res);
        }
      } catch(error) {
        errorHandle(res);
      }
    })
  } else if(req.url == "/todos" && req.method == "DELETE") {
    todos.length = 0;
    res.writeHead(200, headers); 
    res.write(JSON.stringify({
      "status": "success",
      "data": todos,
      "isDelete": "yes"
    }));
    res.end()
  } else if(req.url.startsWith("/todos/") && req.method == "DELETE") {
    const id = req.url.split('/').pop();
    const index = todos.findIndex(ele => ele.id == id);
    if(index !== -1) {
      todos.splice(index, 1);
      res.writeHead(200, headers); 
      res.write(JSON.stringify({
        "status": "success",
        "data": todos,
      }));
      res.end();
    } else {
      errorHandle(res);
    }
  } else if(req.url.startsWith("/todos/") && req.method == "PATCH") { // 新增 PATCH
    req.on('end', ()=> { // 一樣先使用 req.on 來接收傳出去的值
      try { // 若成功則跑 try
        const todo = JSON.parse(body).title; // 獲取 todo 的值
        const id = req.url.split('/').pop(); // 刪除 /，取得最後一筆資料
        const index = todos.findIndex(ele => ele.id == id); // 比對 id，找到索引位置
        if(todo !== undefined && index !== -1) {
          todos[index].title = todo // 利用索引位置 index，賦予 todos.title
          res.writeHead(200, headers); 
          res.write(JSON.stringify({
            "status": "success",
            "data": todos,
          }));
          res.end();
        } 
      } catch {
        errorHandle(res);
      }
    });
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