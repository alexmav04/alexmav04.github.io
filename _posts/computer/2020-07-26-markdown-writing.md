---
title: "[網頁]常用的Markdown語法"
layout: single
author_profile: true
comments: true
categories:
  - computer
tags:
  - markdown
  - website
  - blog
---
大概都參考[Markdown 語法說明](https://markdown.tw/)的內容，這裡用自己的方式記錄我常用的幾項。

線上編輯，可以即時看到結果：  
[stackedit](https://stackedit.io/)、[markdown editor](https://markdown-editor.github.io/)

---

## 標題
H多少就給多少#符號
```
# This is H1

## This is H2

### This is H3
```
以此類推到H6

展示：
# This is H1

## This is H2

### This is H3

---
## 區塊
區塊的背景預設是會有不同的色塊，方便跟其他內容作區隔。首先要認識這個符號：`，這叫「反引號」，通常位在鍵盤的左上角（1的旁邊），不要跟單引號搞混了！

### 小段落
```
`這是一個小段落`
```
展示：  
`這是一個小段落`

### 大段落
```
    前面先空四個格，就可以擁有一個大段落
    上句按Enter會自動和上面有同樣縮排，而且還可以繼續編輯
    廢話，不然叫大段落叫假的喔

要結束的話一樣多一空行就好了。
```
展示：

    前面先空四個格，就可以擁有一個大段落
    上句按Enter會自動和上面有同樣縮排，而且還可以繼續編輯
    廢話，不然叫大段落叫假的喔

要結束的話一樣多一空行就好了。

### 程式碼段落
既然是開發人員會用的工具，程式碼相關的段落當然也不能少，不會有人想看純文字程式吧XD
分別用三個反引號上下包住你的程式碼，後面空一格加上程式語言（可寫可不寫）。
```
  (開頭反引號三個)``` js
        function shivMethods(ownerDocument, data) {
          if (!data.cache) {
            data.cache = {};
            data.createElem = ownerDocument.createElement;
            data.createFrag = ownerDocument.createDocumentFragment;
            data.frag = data.createFrag();
          }
  (結尾反引號三個)```
```

展示：  
``` js
  function shivMethods(ownerDocument, data) {
    if (!data.cache) {
      data.cache = {};
      data.createElem = ownerDocument.createElement;
      data.createFrag = ownerDocument.createDocumentFragment;
      data.frag = data.createFrag();
    }
  }
```

### 階層式段落
```
> wow
>> woow
>> wooow
>>> 看到了嗎
>> 但收不回來
>>>>>> 可以一直延伸但是收不回來
> 終於看開愛回不來
>>>>>>>>> 但繼續縮沒問題的
```
展示：  
> wow
>> woow
>> wooow
>>> 看到了嗎
>> 但收不回來
>>>>>> 可以一直延伸但是收不回來
> 終於看開愛回不來
>>>>>>>>> 但繼續縮沒問題的

通常我們會用一個>當作引言  
```
> 天才是1%的天賦加上99%的努力... 放屁！　-Alex，也就是我
```
展示：  
> 天才是1%的天賦加上99%的努力... 放屁！　-Alex，也就是我

Q: 為什麼我換行了，樣式卻還是一直在？  
A: 因為在markdown中，需要有一個以上的空行它才知道要切割段落。

```
> 天才是1%的天賦加上99%的努力... 放屁！
我只按了一個enter，它以為我還要繼續寫
其實也對啦，花了99%的力氣當專利蟑螂，愛迪生，真有你的！ -Alex，還是我
```
展示：  
> 天才是1%的天賦加上99%的努力... 放屁！
我只按了一個enter，它以為我還要繼續寫
其實也對啦，花了99%的力氣當專利蟑螂，愛迪生，真有你的！ -Alex，還是我

---

## 分隔線
用三個以上的星號、減號或是底線，就可以獲得一個分隔線  
（原本在其他地方都有成功，但在我的網站似乎星號是不成功的）
```
***
**** **** ***
---
- - - 
-------------
```
展示：  
***  
**** **** ***  
---
- - -
-------------

## 關於字的變化
### 強調
（符號後面不用空格）  
```
*斜體*
_這也斜_
阿不就很*斜*
```
展示：  
*斜體*  
_這也斜_  
阿不就很*斜*  

```
**粗體**
__有兩個底線喔__
是不是真的很**粗**呵呵呵
```
展示：  
**粗體**  
__有兩個底線喔__  
是不是真的很**粗**呵呵呵  

在html標籤中，上面斜體的部分就是`<em>`
下面粗體是`<strong>`標籤

### 跳脫字元
跟一般程式寫法一樣，前面加個反斜線
```
\# 這樣就不會變標題了
```
展示：
\# 這樣就不會變標題了

---
## 條列清單
### 項目符號
可以有三個符號，也就是三個層次。
```
* 第一項
    * 縮排第一項，這裡自己tab縮
* 第二項
    * 縮排，第二項中第一小項
    * 縮排，第二項中第二小項
        * 再縮， 第二項中第二小項的第一小小項... 工三小
```
展示：  
* 第一項
    * 縮排第一項，這裡自己tab縮
* 第二項
    * 縮排，第二項中第一小項
    * 縮排，第二項中第二小項
        * 再縮， 第二項中第二小項的第一小小項... 工三小

---
### 數字編號
跟上面的概念差不多，就改成數字而已。上面例子太多字換個實際例子。一樣可以有三層：
```
1. 中式
    1. 早餐
        1. 蘿蔔糕
        2. 煎餃
    2. 午餐
        1. 鐵板麵
        2. 小籠包
2. 西式
    1. 早餐
        1. 熱狗加蛋
        2. 玉米濃湯
    2. 晚餐
        1. 牛排
        2. 突然想不到要寫啥
```
展示：
1. 中式
    1. 早餐
        1. 蘿蔔糕
        2. 煎餃
    2. 午餐
        1. 鐵板麵
        2. 小籠包
2. 西式
    1. 早餐
        1. 熱狗加蛋
        2. 玉米濃湯
    2. 晚餐
        1. 牛排
        2. 突然想不到要寫啥

---
### 核取方塊
說CheckBox可能會比較多人聽過，不會記錄，純粹讓你勾著玩的，橫線後面也有一格空白要注意。
```
- [x] 有勾的
- [ ] 沒勾的
```
展示：
- [x] 有勾的
- [ ] 沒勾的

---
## 連結
### 文字連結
注意：兩個括號間是沒有空格的！
```
[連結的文字](網址)
[我要連去Google]( https://www.google.com.tw/)
```
展示：  
[我要連去Google]( https://www.google.com.tw/)

這裡只是我習慣的連結方式，還有更多方式請參閱：[Markdown 語法說明-連結](https://markdown.tw/#link)

### 圖片
和連結很像，前面多個驚嘆號。
```
![圖片的替代文字](圖片網址或路徑)
![這是一張示範圖](https://raw.githubusercontent.com/alexmav04/alexmav04.github.io/master/_posts/computer/img/mingw-w64-install/mingw-w64-install-02.JPG)
```
展示：
![這是一張示範圖](https://raw.githubusercontent.com/alexmav04/alexmav04.github.io/master/_posts/computer/img/mingw-w64-install/mingw-w64-install-02.JPG)

---
## 後記：
最後要為我的粗神經自首。  
前情提要：我平常習慣在Evernote上面作筆記。當我在寫這篇文章時有一種莫名的熟悉感－－幹原來之前覺得雞婆的設定就是小小的Markdown標記啊！！  
那時我還不知道Markdown是啥，自己又有很多詭異的寫文習慣，一直很不能適應打個米字號一直給我轉成項目符號，打個空白的中括號也要轉為一個空白方塊，覺得雞婆就把設定關了。  
難怪寫文時覺得莫名熟悉，突然很愧疚自己竟然隨意批評人家雞婆XDD 網路上也很多關於Evernote加上Markdown的編輯方式，如果有興趣的可以去找一下。

---
## 參考  
♦ [Markdown 語法說明](https://markdown.tw/)