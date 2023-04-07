---
title: Node.js 學習日記(2) - NPM
date: 2023-01-16 11:03:20
tags: Node.js
categories: Node.js
---
Node.js 學習日記 - NPM
<!--more-->

### 建立 package.json
```
npm init 建立檔案
```

接著會詢問以下問題：
- package name: Project 要叫什麼名字
- version: Project 現在該是第幾版
- description: Project 基本介紹
- entry point: 進入點，Project 應該要執行哪個檔案
- author: 作者(自己)
- license: Project 是採用什麼授權的
- test command: 這個不太重要

產生出來的 package.json 會長這樣
```js
{
  "name": "node",
  "version": "1.0.0",
  "description": "",
  "main": "app.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1"
  },
  "author": "",
  "license": "ISC"
}
```
**************

### NPM 安裝套件流程
可以先到 NPM [官方網站](https://www.npmjs.com/)，在紅框處可以直接收尋套件名稱

![](/images/Node01.png)

一樣先 npm init 建立 package.json，然後安裝 npm install express
```js
{
  "name": "node",
  "version": "1.0.0",
  "description": "",
  "main": "app.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1"
  },
  "author": "",
  "license": "ISC",
  "dependencies": {
    "express": "^4.18.2"
  }
}
```
完成的話在 package.json 就會看到 express 了

最後在 app.js 進行 require
```
var express = require('express')
console.log(express)
```
若執行出來有各式指令語法，代表成功了

**************

### 本版號說明
例如：jQuery v1.12.0
- 1: 主要版本號
- 12: 次要版本號
- 0: bug 修正

- ^安裝 1.x.x
- ~安裝 1.12.x
- latest 安裝最新的

**************

### npm i -save、--save-dev、-g 差異
- -save Node 應用程式上線會用到的 npm
- --save-dev Node 開發測試用的 npm
- -g 安裝在電腦的全域環境

**************

### nodemon 套件 - 執行 NPM 內容流程

執行安裝：
```
npm install nodemon -g
```

使用：
```
nodemon app.js
```
接著就會自動更新 js 裡面的內容，就不需要重新啟動哩

**************

### 安裝 uuid
執行安裝：
```
npm i uuid --save
```

執行 require 載入：
```js
const { v4: uuidv4 } = require('uuid')
const a = uuidv4()

console.log(a)
// 97d2d35d-5d17-4608-907e-bc6fde4fdf2c
```