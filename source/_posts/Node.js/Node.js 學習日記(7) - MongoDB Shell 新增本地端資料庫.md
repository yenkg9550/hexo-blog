---
title: Node.js 學習日記(7) - MongoDB Shell 新增本地端資料庫
date: 2023-03-11 17:27:58
tags: Node.js
categories: Node.js
---
Node.js 學習日記(7) - 新增本地端資料庫
<!--more-->

[簡報介紹](https://whimsical.com/mongodb-FCuHtC9DW1SHqRWBKsJKp1)

一開始先開啟終端機設定好 db 的資料夾路徑：
```
mongod --dbpath /Users/jianzhi/Desktop/Desktop/後端/mongoDB/data
```

設定好之後，在執行 mongo 開啟資料庫

**************

可以使用 show dbs 查看資料庫內容：
```
> show dbs
admin   0.000GB
config  0.000GB
local   0.000GB
```

**************

輸入 use hotel 切換到 hotel 的 collection
```
> use hotel
switched to db hotel
> show dbs
admin   0.000GB
config  0.000GB
local   0.000GB
```

**************

輸入 db.rooms.insertOne 新增房型資料
```
> db.rooms.insertOne({"rating":4.5, "price": 1000, "name":"標準單人房"})
```
成功之後訊息：
```
{
	"acknowledged" : true,
	"insertedId" : ObjectId("640c492f7f524b8392c70008")
}
> db.rooms.find()
{ "_id" : ObjectId("640c492f7f524b8392c70008"), 
"rating" : 4.5, "price" : 1000, "name" : "標準單人房" }
> 
```

成功之後再輸入 show dbs 就會看到 hotel 的資料囉！

