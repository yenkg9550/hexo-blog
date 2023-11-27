title: Node.js 學習日記(11) - MongoDB Shell - 尋找資料
tags:
  - Node.js
categories:
  - Node.js
date: 2023-03-15 23:03:00
---
Node.js 學習日記(11) - 尋找資料

<!-- more -->

### 尋找單筆資料 findOne
```js
db.r
```
************

### 尋找多資料 find
```js
db.rooms.find({
  {
    "price": 2500
    "rating": 4
  }
})
```