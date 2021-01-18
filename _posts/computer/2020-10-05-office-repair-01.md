---
title: "[Office]Excel的XLLEX.DLL毀損以及Word檔頁碼消失"
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
微軟的東西總是這樣，平常用的小功能好好的，某天一更新就東缺西缺（印象中更新和Office明明是兩回事啊！）。支援的客服也很爛，只會跳針叫大家重新安裝，就跟遙控器不靈就猛力敲打他一樣。但至少敲遙控器會有反應，這次Office我大概用各種不同的方式重裝3次，都一樣沒有用。接下來這個方式全都是仰賴中國人的百度，雖然心裡很常幹譙中國人，但老實說什麼奇奇怪怪的問題找簡體字都有解，不愧是占了全世界五分之一人口的龐大資料庫啊……

# Excel的XLLEX.DLL毀損

![office-repair-01.JPG](https://raw.githubusercontent.com/alexmav04/alexmav04.github.io/master/_posts/computer/img/office-repair-01.JPG)

某天要打開Excel就跳出這個視窗了。原本以為是特定檔案毀損，但單開Excel也是這種情形。

## 解決方法

找了很多方式，最後這篇討論串其中一個方案對我的電腦是有用的。開啟之後要展開看看其他回答，會有很多光怪陸離的解法（可以注意到他們會非常強調在他們電腦的解法不一定適用別的電腦）

[EXCEL词典（XLLEX.dll)文件丢失或损坏？](https://www.zhihu.com/question/23932842)

把 C:\Program Files\Microsoft Office\root\Office16\1028 中的  
**XLLEX.DLL**  
複製到 C:\Windows\System32

網路上有些會提供下載XLLEX.DLL的檔案，但我的Office是2019，剛開始找不太到。也多虧找不到，才知道原來自己電腦就有了（不然原本也不可能正常）。所以如果要用這種方式，建議搜尋自己電腦是否有這個檔案，再移去適當的資料夾。

# Word檔頁碼消失
這個也是有些頭痛的問題，雖然Word可以開，但沒了那些小功能非常不便利。

![office-repair-02.JPG](https://raw.githubusercontent.com/alexmav04/alexmav04.github.io/master/_posts/computer/img/office-repair-02.JPG)

## 解決方法

後來參考了這篇文章：[Word添加页码时页面底端没有怎么办?](https://www.zhihu.com/question/396454319)

雖然沒有很直接解決掉我的問題，但至少知道原因：原本該有的模板消失了。剛好上個Excel的問題也是差不多同一時間發生的，所以衍生出了這個想法：這模板我電腦裡應該是有的，或許也只要移到適當的資料夾就可以了？

把 C:\Program Files\Microsoft Office\root\Office16\Document Parts\1028\16 中的  
**Built-In Building Blocks.dotx**  
複製到 C:\Users\我自己的名字\AppData\Roaming\Microsoft\Document Building Blocks

順帶一提，這個資料夾之前是完全空的，我對於原因完全沒有頭緒。

## 突然出現個小問題
頁碼的功能是成功出現了，但是有個小問題：原本該是數字的頁碼變成 {PAGE \* MERGEFORMAT}

[如何處理 Word 文件中的頁碼變成 {PAGE \* MERGEFORMAT}](https://support.microsoft.com/zh-tw/help/2791384)中所說：  
您設定到了【顯示功能變數代碼代替數值】的功能，此選項可在文件中顯示功能變數代碼，而非功能變數結果，因此我們只要取消該功能就可以了。

於是直接用Alt+F9解決，或是透過變更Word選項>進階功能取消勾選。

---
這次沒有後記也沒參考（其實都在上面了），我好累T-T
