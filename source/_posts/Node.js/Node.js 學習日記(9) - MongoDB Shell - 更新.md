---
title: Node.js 學習日記(9) - MongoDB Shell - 更新資料
date: 2023-03-11 18:17:59
tags: Node.js
categories: Node.js
---
Node.js 學習日記(9) - 更新資料：updateOne、updateMany

<!-- more -->

### 修改單筆資料
利用 id 尋找單筆資料
```
db.rooms.updateOne(
  {
    "_id": ObjectId("640c492f7f524b8392c70008")
  },
  {
    "$set": {
      "name": "標準單人升級房",
      "rating": 5.0
    }
  }
)
```

******

### 修改多筆資料

尋找評價 4.5 顆星，並且改為 1 顆星
```
db.rooms.updateMany(
  {
    "rating": 4.5
  },
  {
    "$set": {
      "rating": 1
    }
  }
)
```

********

### 更新資料 - replaceOne

功能跟 updateOne 很像，但是整個資料會進行替換 ~
例如原本這筆資料有：
```
_id：640c492f7f524b8392c70008
rating：5
price：1000
name："標準單人升級房"
```

使用 db.rooms.replaceOne
```
db.rooms.replaceOne(
  {
    "name": "標準單人升級房"
  }, 
  {
    "name": "總統套房"
  }
)
```
最後會只有留下 "name": "總統套房"