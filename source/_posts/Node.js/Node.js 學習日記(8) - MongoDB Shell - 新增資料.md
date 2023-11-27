title: Node.js 學習日記(7) - MongoDB Shell - 各項指令
tags: Node.js
categories: Node.js
date: 2023-03-11 17:52:19
---
Node.js 學習日記(7) - 新增資料：insertOne、insertMany

<!-- more -->

### 新增單筆資料 - insertOne 
使用 db.rooms.insertOne 新增一筆資料

```js
> db.rooms.insertOne({"rating":4.5, "price": 1000, "name":"標準單人房"})
```

******

### 新增多筆資料 - insertMany
若要新增多筆資料到同一個陣列裡面，可以使用 insertMany
```
db.rooms.insertMany([
  {"rating":4.5, "price": 1000, "name":"標準單人房"},
  {"rating":4.5, "price": 1500, "name":"高級單人房"}
])
```
******
### 修改單筆資料 - updateOne
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

### 修改多筆資料 - updateMany

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

********

### 刪除單筆資料 - deleteOne
```
db.rooms.deleteOne(
  {
    "_id": ObjectId("640c492f7f524b8392c70008")
  }
)
```
******

### 刪除多筆資料 - deleteMany

```
db.rooms.deleteMany(
  {
   "price": 1000
  }
)

************

```
### 尋找單筆資料 - findOne
```js
db.rooms.find({
  {
    "price": 2500
  }
})
```

************

### 尋找多資料 - find
```js
db.rooms.find({
  {
    "price": 2500
    "rating": 4
  }
})
```