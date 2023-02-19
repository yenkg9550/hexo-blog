---
title: Node.js 學習日記(5) - heroku 部署環境
date: 2023-01-20 01:21:27
tags: Node.js
categories: Node.js
---
Node.js 學習日記(5) - heroku 部署環境
<!--more-->

### 加入 .gitignore
```
node_modules/
```
加入這個主要是不要將 node_modules 部署上去

**********

### heroku 環境設置

先回到 server.js 修改一下 port
```js
// server.js
server.listen(process.env.PORT || 3005); // 新增 process.env.PORT
```

接著到 package.json
```json
{
  "name": "node",
  "version": "1.0.0",
  "description": "",
  "main": "app.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1",
    "start": "node server.js" // 加入這段，原因是 heroku 只認得這段來運行
  },
  "author": "",
  "license": "ISC",
  "dependencies": {
    "express": "^4.18.2",
    "uuid": "^9.0.0"
  },"engines": { // 加入 node 版本號
    "node": "18.x"
  }
}
```

**********

### heroku 部署流程

1. 先安裝 heroku 全域環境：
```
npm i -g heroku
```

2. 測試是否有安裝成功：
```
heroku --version
```

3. 登入 heroku：
```
heroku login
```

4. 建立 app
```
heroku create
```

5. 推上 heroku
```
git push heroku master
```

6. 打開 heroku page
```
heroku open
```

到這邊就部署完畢了 ~
若有出現錯誤可以到 heroku log 查看錯誤訊息

**********


<font color=#FF0000>另外要特別注意 2022 年 11 月 28 日 heroku 不在提供免費</font>

(下圖為 2023/01/20 的收費方式)
![](/images/Node03.png)

可以參考[這篇文章](https://israynotarray.com/other/20221213/3036227586/)，轉移到 Render 進行免費部署