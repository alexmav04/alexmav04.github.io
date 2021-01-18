---
title: "[JavaScript] 在function前加一個驚嘆號是什麼意思"
layout: single
author_profile: true
comments: true
categories:
  - computer
tags:
  - JavaScript
sidebar:
  nav: "sidepost"
---
蠻常看到JavaScript中在function前加一個驚嘆號，理解用途後蠻容易懂的，故先做個筆記放著。

---

一般JavaScript的function有兩種寫法：
第一種：
```js
funtion fnAAAA() {
    alert('msg');
}
```

第二種：
```js
var fnAAAA = function() {
    alert('msg');
}
```

要呼叫就是fnAAAA()

如果要立刻呼叫的話，直覺會想這樣寫：
```js
funtion fnAAAA() {
    alert('msg');
}()
```
但這樣是不可行的。

其實只要把適當的東西包在括號裡面就好：
```js
(
    function() {
        alert('msg');
    }
)()
```

像把function框起來整個執行，機器也比較好懂的感覺
而驚嘆號是較精簡的寫法（其實也才省一個符號XD）
```js
!function() {
    alert('msg');
}()
```
這樣函數宣告完也就能立即執行了