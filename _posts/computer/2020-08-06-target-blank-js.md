---
title: "[網頁]強制連結開新的分頁"
layout: single
author_profile: true
comments: true
categories:
  - computer
tags:
  - jekyll
  - website
  - JavaScript
sidebar:
  nav: "sidepost"
---
先說，我這篇完完全全是照這篇文章：[Jekyll 新分頁開啟超連結](https://note.pcwu.net/2017/02/05/jekyll-link-new-tab/)  
之前太習慣wordpress簡單的介面設定，碰到jekyll這種不熟悉的工具連這個小功能都得求助XD

## 為什麼要多新的分頁
不論是何種型態的網站，大部分的站長都會希望網友們能夠在自己的地盤待久一點吧。但有個惱人的狀況是，一旦多了連結，沒有特別去設定的話，整個網頁就跳轉到別的地方了。萬一網友也在這個跳轉出去的網頁久待，很快的他對你的網頁就只是個過客。或許也有網友原本計畫繼續看，但中間有個連結所以就點進去了，很可能不小心看完手殘就把整個網頁關掉（我就是那個網友）。  
當然還有其他更多的因素，較常見的是有放廣告，為了增加廣告商們的曝光率，最簡單的方式就是讓網友們久留，如此也才能賺到錢。

## 讓網頁中的連結都開新分頁
在WordPress中要開新分頁很簡單，WordPress看你使用的編輯器，通常食指動一動就設好了。  
若是自己寫的程式碼，就多一個步驟，像是HTML的`<a>`標籤，後面再加上 `target="_blank"`，

例如 `<a href="google.com" target="_blank"></a>`，而js的按鈕寫法就是 `<button type="button" onclick="window.open('google', '_blank')">BUTTON</button>`

用Jekyll這工具生成的網頁做法也很類似，像我是用markdown寫的文章，只要在連結後面加上`{:target="_blank"}`即可

`[link][url]{:target="_blank"}`

但是，當初會想用這種筆記部落格形式，最大的初衷就是想省事，每次加個reference都要記得加一次`{:target="_blank"}`也是挺煩的，所以就用了網友的方法。（我是完完全全照做喔，建議可以直接看原文章）
### 新增一個javascript檔案
用javascript弄個可以開新分頁的檔案，放在/asserts/js/裡面，取名叫做new-tab.js

```js
function handleExternalLinks () {
  var host = location.host
  var allLinks = document.querySelectorAll('a')
  forEach(allLinks, function (elem, index) {
    checkExternalLink(elem, host)
  })
}

function checkExternalLink (item, hostname) {
  var href = item.href
  var itemHost = href.replace(/https?:\/\/([^\/]+)(.*)/, '$1')
  if (itemHost !== '' && itemHost !== hostname) {
    item.target = '_blank'
  }
}

// NodeList forEach function
function forEach (array, callback, scope) {
  for (var i = 0; i < array.length; ++i) {
    callback.call(scope, array[i], i)
  }
}

handleExternalLinks ()
```
### 加入剛剛寫的檔案
既然生了他，就要用他（咦？）。  
我們要在 `_layout/default.html` 中引入這個javascript檔，有在寫網頁的人對這個步驟應該不陌生。至於為什麼是default.html，是因為沒意外的話他應該是Jekyll整個網站會用到的模板（不太確定這解釋對不對），不然直接去找自己的檔案看看版面都怎麼設置的應該也是很好懂。所以在default.html引入，整個網站都會執行。

至於加在<body>還是<head>... 這個部份我還不是太懂，或許寫夠多了才會理解，到時再寫篇文章。目前我是粗略地分：沒有要立刻執行的會加在<head>，要立刻執行的會加在<body>的尾端。
```html
<html>
  <head>    ......    </head>
  <body>
  ......
  <script type="text/javascript" src="/assets/js/new-tab.js"></script>
  </body>
<html>
```
## 微解析
有鑑於直接複製貼上太廢了，既然程式短短的，還是稍微用我乾癟的腦子生個解釋吧。

這裡的概念很簡單，要抓取所有的`<a>`標籤，加上 `target="_blank"`，但同時我不想要每個連結都要生一個分頁，希望能夠過濾自家的文章（不然每點一個文章就要生一個分頁，發瘋喔），所以這裡原作者設計了兩個funciton：  
handleExternalLinks: 這個function要處理所有收集到的外部
linkcheckExternalLink: 這個function要用來判斷是不是自己家的link，不是的話再開分頁

關於forEach那個function，我似懂非懂，主要是不太知道callback用法，之後再回來補足這坑。

```js
function checkExternalLink (item, hostname) {
  var href = item.href  //這是要取得url
  var itemHost = href.replace(/https?:\/\/([^\/]+)(.*)/, '$1')
//利用正規表示式和replace去抓取我要的host（要等下判斷）

//接著判斷：hostname不是空的且和網站本身的hostname不一樣
//如果條件都符合，表示連結必須要開新的分頁

  if (itemHost !== '' && itemHost !== hostname) {
    item.target = '_blank'
  }
}
```
這部分寫完，包在handleExternalLinks中執行
```js
function handleExternalLinks () {
//先取得網站本身的hostname和所有<a>標籤內容
//(location.host 可以取得目前造訪網頁的主機名稱)
  var host = location.host
  var allLinks = document.querySelectorAll('a')
  forEach(allLinks, function (elem, index) {
    checkExternalLink(elem, host)  //這裡就是剛寫好要做判斷的function
  })
}
```
最後要立刻執行，記得加上：
```js
handleExternalLinks ()
```

---

## 參考
♦ [Jekyll 新分頁開啟超連結](https://note.pcwu.net/2017/02/05/jekyll-link-new-tab/)  
♦ [如何用javascript取得網址(URL)](https://medium.com/@chienrongkhor/%E5%A6%82%E4%BD%95%E7%94%A8javascript%E5%8F%96%E5%BE%97%E7%B6%B2%E5%9D%80-url-d0c69934d2a9)