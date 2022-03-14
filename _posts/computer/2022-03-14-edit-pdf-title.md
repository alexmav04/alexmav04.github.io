---
title: "[PDF]Microsoft to PDF的標題名和檔名不同"
layout: single
author_profile: true
comments: true
categories:
  - computer
tags:
  - Microsoft
  - PDF
sidebar:
  nav: "sidepost"
---
PDF文件可跨平台、不會變動格式、可跨設備等等優點，讓這種檔案格式變得相當廣泛，現在傳個資料，甚至自己準備資料開會時，幾乎都會準備好pdf檔，以避免任何例外狀況發生。

但是近期在下載自己先前轉好的pdf檔時，發現一個小問題：當初上傳的檔名和下載的檔名完全不符合。不知道大家有沒有這個感覺，好像有時候我們下載的檔案名稱似乎根本沒對上內容？但因為下載時檔名可以再更改，所以倒也沒有太在意。

## 悲劇發生
直到我有次上傳檔名為"problem1.pdf"的檔案，再次下載檔預設卻變成"ppp123fuckyou.pdf"，嘿對我超常用髒話命名暫存檔，依我自己的習慣比較不會誤傳一個不完整的檔案。但這份文件是要給學生看的，並且這個平台不會出現檔案對話框，而是直接下載到電腦裡，所以這突然變成一個超嚴重的問題啊！不知道是在哪個儲存步驟直接就幫我定義了pdf檔的標題。

## PDF標題和檔名差別在哪？
參考了一些說明，發現PDF文件有關於「文件描述」的設定。檢視這項設定的路徑為**檔案** \\(\to\\) **內容**，對話框開啟就可以看見那個萬惡的標題了。

![edit-pdf-title-001](https://i.imgur.com/dRaY9Vq.jpg)

![edit-pdf-title-002](https://i.imgur.com/SMvOAAi.jpg)

現在蠻多人都直接使用Microsoft to PDF的功能，很方便但很少人會注意到這個小地方，導致收文件的另一方下載時一頭霧水。像是一個文件的版次可能已經改變，由ver1轉換到ver6，檔名也改好了上傳，殊不知下載的檔名並非我們預期的那樣，或是甚至出現亂碼。有的時候檔名可能也沒什麼問題，但使用Chrome去預覽的時候又會原形畢露，很討厭。

![edit-pdf-title-003](https://i.imgur.com/2u7mOgU.jpg)

## 改標題的幾種方法

### Adobe Acrobat Pro
其實如果出問題的pdf檔案是自產的，改標題相對來說是相當容易的一件事情。但若檔案不是自產的，那麼可能就只能用Adobe Acrobat Pro去改了。

啊因為軟體很貴，我又沒錢，所以這部份沒圖也沒說明哈哈哈哈。我猜應該也是在**檔案** \\(\to\\) **內容**之類的路徑裡面，能夠直接去修改。

### Microsoft to PDF
這應該算是很多人轉換pdf的方式，改標題的方式也相當簡單。

當你製作好文件，要列印或是轉換成pdf之前，先進入**檔案**的頁面，再至**資訊**的頁籤查看標題，更改你要的標題，通常匯出後pdf的標題應該就會是你自己設定好的標題了。

![edit-pdf-title-004](https://i.imgur.com/EWa5YVU.jpg)

如果你只有pdf檔，沒有原始轉pdf的那個文件的話，有個反向作法：搜尋官方的線上功能把pdf先轉Word檔，這樣就可以執行到資訊頁面改標題的功能了。

### 使用NotePad++
這個算是我最驚訝的一招。一直以來都很喜歡用NotePad++解決一些很棘手的問題，可能是功能和介面很簡單，限制也不多，有些複雜問題在NotePad++反而會變得相當容易。不過這次完全沒想到連pdf都可以有辦法去變動。

下載NotePad++後，針對要修正的PDF檔點按右鍵，使用NotePad++開啟檔案。

![edit-pdf-title-005](https://i.imgur.com/S4gPpl2.jpg)

在NotePad++編輯器中，使用鍵盤**ctrl+F**或是點按功能列的**搜尋**，並搜尋"title"。

![edit-pdf-title-006](https://i.imgur.com/AMGYqiM.jpg)

可以找到Title標籤，然後看到亂七八糟的標題XD

![edit-pdf-title-007](https://i.imgur.com/8qoGL2t.jpg)

把括號內的文字改成自己想要的標題。

![edit-pdf-title-008](https://i.imgur.com/7Y1aogf.jpg)

要特別注意的是除了這個之外不能動到任何東西！！改好之後再儲存即可。這樣使用前述方式查看標題，就可以很舒爽地看到自己的檔案終於回歸正軌了！不過有趣的是，執行完這個動作後，這個檔案有點像是變成「草稿」已經被變動過的感覺，打開之後再關掉，它會問你要不要儲存這個檔案。

![edit-pdf-title-009](https://i.imgur.com/yARlo7b.jpg)

而儲存過後再用NotePad++打開，內容會完全不一樣，搜尋"title"只能找到這串文字。

![edit-pdf-title-010](https://i.imgur.com/FrnGhQp.jpg)

而改掉這段文字後，反而會出現檔案損壞的對話框，感覺好像什麼東西被鎖定了。

![edit-pdf-title-011](https://i.imgur.com/Qmw3Mqu.jpg)

因為不太清楚pdf格式的原理，所以這種狀況目前對我來說無解。以上是三種我覺得比較常用的方式，供大家參考。

## 參考資料
* [How to Change the Document Title of a PDF](https://techswift.org/2021/08/26/how-to-change-the-document-title-of-a-pdf/)
* [PDF附件於網頁檢視顯示名稱與實際檔名不同?](https://lis.nsysu.edu.tw/p/406-1001-208511,r4133.php?Lang=zh-tw)
