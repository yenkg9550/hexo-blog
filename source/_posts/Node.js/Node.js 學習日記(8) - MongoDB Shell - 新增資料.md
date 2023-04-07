---
title: Node.js 學習日記(8) - MongoDB Shell - 新增資料
date: 2023-03-11 17:52:19
tags: Node.js
categories: Node.js
---
Node.js 學習日記(8) - 新增資料：insertOne、insertMany

<!-- more -->

### 新增單筆資料
一樣先使用 db.rooms.insertOne 新增一筆資料

```js
> db.rooms.insertOne({"rating":4.5, "price": 1000, "name":"標準單人房"})
```

******
### 新增多筆資料
若要新增多筆資料到同一個陣列裡面，可以使用 insertMany
```
db.rooms.insertMany([
  {"rating":4.5, "price": 1000, "name":"標準單人房"},
  {"rating":4.5, "price": 1500, "name":"高級單人房"}
])
```