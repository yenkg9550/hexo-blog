---
title: Node.js 學習日記(6) - 非同步技巧
date: 2023-01-28 17:20:59
tags: Node.js
categories: Node.js
---
Node.js 學習日記(6) - 非同步技巧
<!--more-->

### 批改學生考卷，並給予獎勵：
- 步驟一：批改成績
- 步驟二：確認獎品
```js
function correctTestAndcheckReward() {
  console.log('批改成績中')
  setTimeout(() => {
    const score = Math.random()*100;
    console.log(`成績是 ${score}`)
    setTimeout(() => {
      if (score >= 90) {
        return("獲得獎勵");
      } else if (score > 60 && score < 90) {
        return("嘉獎")
      } else {
        return("毒打")
      }
    }, 1000)
  }, 1000)
}

const a = correctTestAndcheckReward()
console.log(a)

/* 結果如下：
批改成績中
undefined --> 因為非同步關係，因此 a 是 undefined
成績是 71.9082792303825
*/
```

**************

### setTimeout 語法
```js
const timeout = setTimeout(callFunction, 3000)
function callFunction() {
  console.log('觸發')
}

// 三秒後顯示觸發
```
箭頭函式寫法：
```js
const timeout = setTimeout(() => console.log("觸發"), 3000)
```

**************

### 建構 promise
```js
const checkScore = new Promise((resolev, reject) => {
  console.log("正在批改中")
  setTimeout(() => {
    const score = Math.round(Math.random() * 100);
    if (score >= 60) {
      resolev(score);
    } else {
      reject("不及格")
    }
  }, 1000)
})

checkScore
  .then(data => console.log(data)) /* 成功 resolev 會跑 .then */
  .catch(error => console.log(error)) /* 失敗 reject 會跑 .catch */
```

**************

### promise 帶參數寫法

```js
const checkScore = (score) => {  // 宣告一個參數
  return new Promise((resolev, reject) => { // return new Promise
  console.log("正在觀察是否及格")
    setTimeout(() => {
      if (score >= 60) {
        resolev(score);
      } else {
        reject("不及格")
      }
    }, 1000)
   })
}

checkScore(80) // 帶上 80 的值
  .then(data => console.log(data))
  .catch(error => console.log(error))

/*
正在觀察是否及格
80
*/
```

******

### 重構批改作業邏輯

```js
function correctTestAndcheckReward(name) {
  return new Promise((resovle, reject) => {
    console.log("批改作業中");
    setTimeout(() => {
      const score = Math.round(Math.random() * 100);
      resovle({
        name,
        score
      })
    }, 2000)
  })
}
correctTestAndcheckReward("小明")
.then(data => console.log(data))

/* 批改作業中
{ name: '小明', score: 55 }
*/
```

******

### 撰寫 catch 流程

```js
function correctTestAndcheckReward(name) {
  return new Promise((resovle, reject) => {
    console.log("批改作業中");
    setTimeout(() => {
      const score = Math.round(Math.random() * 100);
      if (score >= 60) {
        resovle({
          name,
          score
        })
      } else {
        reject("您已達退學門檻")
      }
    }, 2000)
  })
}
correctTestAndcheckReward("小明")
.then(data => console.log(data))
.catch(error => console.log(error)) // 補上 .catch 接收 reject
```

******

### promise chain 寫法
```js
function correctTestAndcheckReward(name) {
  return new Promise((resovle, reject) => {
    console.log("批改作業中");
    setTimeout(() => {
      const score = Math.round(Math.random() * 100);
      if (score >= 60) {
        resovle({
          name,
          score
        })
      } else {
        reject("您已達退學門檻")
      }
    }, 2000)
  })
}
function checkReward(data) { // 新增 checkReward
  return new Promise((resolve, reject) => {
    console.log("正在檢查獎品中");
    setTimeout(() => {
      if(data.score >= 90) {
        resolve(`${data.name}獲得電影票`)
      } else if (data.score >= 60 && data.score < 90) {
        resolve(`${data.name}獲得嘉獎`)
      }
    }, 1000)
  })
}
correctTestAndcheckReward("小明")
.then(data => checkReward(data)) // 執行 checkReward
.then(name => console.log(`名字是 ${name}`)) // 接收 checkReward resolve 
.catch(error => console.log(error))
```