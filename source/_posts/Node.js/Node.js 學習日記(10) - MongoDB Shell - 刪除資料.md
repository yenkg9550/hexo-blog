---
title: Node.js 學習日記(10) - MongoDB Shell - 刪除資料
date: 2023-03-11 22:50:56
tags: Node.js
categories: Node.js
---
Node.js 學習日記(10) - 刪除資料：deleteOne、deleteMany

<!-- more -->

### 刪除單筆資料
```
db.rooms.deleteOne(
  {
    "_id": ObjectId("640c492f7f524b8392c70008")
  }
)
```
******

### 刪除多筆資料

```
db.rooms.deleteMany(
  {
   "price": 1000
  }
)
```
