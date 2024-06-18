---
title: Vue.js 特殊魔法 - 強制渲染畫面 this.$forceUpdate()
date: 2023-04-07 20:07:56
tags: Vue.js
categories: Vue.js
---

Vue.js 特別知識(1) - 強制渲染畫面 $forceUpdate()
<!-- more -->

## 問題描述：
使用 Vue 框架開發時，在某個 function 片段中的 this.driving_track 加入新的值
```js
this.geocoder.geocode({ location: latlng })
.then((res) => {
  this.driving_track[index].address = res.results[0].formatted_address
}).catch((error) => {
  console.log(error)
})
```

但是在畫面上卻無任何變化（沒有連動）
地址欄位正常要有資料

![](/images/vue_image/vue_image01.png)

## 解決方式：
這時只需要加入 this.$forceUpdate() 強制更新即可（因為數據層太多，需要手動更新）
```js
this.geocoder.geocode({ location: latlng })
.then((res) => {
  this.driving_track[index].address = res.results[0].formatted_address
  this.$forceUpdate()
}).catch((error) => {
  console.log(error)
})
```

<style>
  .custom-class {
    color: blue;
    font-weight: bold;
  }
</style>