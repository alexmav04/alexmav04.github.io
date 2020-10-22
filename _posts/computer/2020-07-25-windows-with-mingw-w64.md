---
title: "[C++]MinGW-w64安裝與設定"
layout: single
author_profile: true
comments: true
categories:
  - computer
tags:
  - C
  - C++
  - MinGW-w64
  - environment
sidebar:
  nav: "sidepost"
---
此篇是使用VScode並運作C和C++程式的環境設置紀錄。  
之前學習程式、使用程式或是在工作上，都有人手把手教學如何設置環境，或是早就已經架好環境。近期是什麼都自己來，發現原來這件事不是想像中那麼簡單（不過對比早期設置好像也省略不少步驟）。有點像是在電腦建造自己的工作室一樣，非常有趣。

## MinGW-w64
我記得使用Linux系統時並不需要安裝任何東西，就可以開始編譯我的程式，不過也許是我記錯了。所以這次是我第一次聽到MinGW-w64這個名字，全名是Minimalist GNU on Windows。我的理解是MinGW-w64是要在Windows環境下使用gcc的方式（簡短來說Windows版的gcc）。

這樣聽起來那些IDE應該是有把MinGW-w64這東西封裝起來了？總而言之，MinGW-w64是一套免費、開源的軟體，是很穩定且可靠的編譯器。

### 下載
我是到SourceForge.net中[MinGW-w64 - for 32 and 64 bit Windows](https://sourceforge.net/projects/mingw-w64/files/mingw-w64/mingw-w64-release/)這個頁面。

![mingw-w64-install-01.JPG](https://raw.githubusercontent.com/alexmav04/alexmav04.github.io/master/_posts/computer/img/mingw-w64-install/mingw-w64-install-01.JPG)


在頁面中可以看到裡面有相當多的檔案。拉到這些檔案底下有個MinGW-W64 Online Installer的項目，我是用這個下載的。

![mingw-w64-install-02.JPG](https://raw.githubusercontent.com/alexmav04/alexmav04.github.io/master/_posts/computer/img/mingw-w64-install/mingw-w64-install-02.JPG)

### 安裝
下載後開始安裝程序。以往遇到這種安裝程序都會開啟無腦模式全部都照默認XD 不過這次要稍微改一些設置。

![mingw-w64-install-03.JPG](https://raw.githubusercontent.com/alexmav04/alexmav04.github.io/master/_posts/computer/img/mingw-w64-install/mingw-w64-install-03.JPG)

Version　是gcc的版本，這裡我選擇默認選項，應該會是最新或是最穩定的。

Architecture　是電腦系統的位元，我的是64bit所以選擇x86_64。

Threads　這個我比較沒概念，Windows似乎就是選win32，其他系統就選posix。

Exception　是選擇異常處理模型，64位元的電腦有兩個選項可以選，這裡我選擇seh。性能較好，但不支持32位元。

Build revision　這裡沒得選擇，就維持默認。

更改完後下一步會進入安裝目錄設置，我是維持默認設定。再來就直接安裝，安裝完應該可以看到一個新的資料夾就叫做mingw-w64。

勇敢地點進去看看內容，在bin資料夾中就可以發現有很多不同的程式，這些都是編譯工具，包含我朝思暮想的gcc和g++等等。這個資料夾等一下會用到，把這裡的目錄複製起來。

![mingw-w64-install-04.JPG](https://raw.githubusercontent.com/alexmav04/alexmav04.github.io/master/_posts/computer/img/mingw-w64-install/mingw-w64-install-04.JPG)

### 設定環境變數
安裝完並不是就結束了，還有環境變數要設定。

找到「本機」按右鍵點選「內容」（總而言之就是能看系統設置的地方），打開後點選「進階系統設定」，開啟以下視窗。

![mingw-w64-install-05.JPG](https://raw.githubusercontent.com/alexmav04/alexmav04.github.io/master/_posts/computer/img/mingw-w64-install/mingw-w64-install-05.JPG)

點選「環境變數」後，找到系統變數中的PATH變數，點選之後按編輯。

![mingw-w64-install-06.JPG](https://raw.githubusercontent.com/alexmav04/alexmav04.github.io/master/_posts/computer/img/mingw-w64-install/mingw-w64-install-06.JPG)

![mingw-w64-install-07.JPG](https://raw.githubusercontent.com/alexmav04/alexmav04.github.io/master/_posts/computer/img/mingw-w64-install/mingw-w64-install-07.JPG)

開啟編輯環境變數的視窗，這裡要按新增，並把剛剛複製的bin資料夾位置貼上去。

![mingw-w64-install-08.JPG](https://raw.githubusercontent.com/alexmav04/alexmav04.github.io/master/_posts/computer/img/mingw-w64-install/mingw-w64-install-08.JPG)

照網路上的教學，到這裡測試應該是ok。但不知為何我怎麼測就是不行，於是參考了另外一位大神的文章，多設置了一項東西，那就是mingw-w64的實際安裝位置。

![mingw-w64-install-09.JPG](https://raw.githubusercontent.com/alexmav04/alexmav04.github.io/master/_posts/computer/img/mingw-w64-install/mingw-w64-install-09.JPG)

設定完後記得按確定。剛剛提到測試，打開Window命令提示字元（搜尋cmd），在視窗中隨便輸入下面其中一個指令，如果出現一大串東西表示設置成功，如果出現「window下g++ 不是内部或外部命令」之類的，就要重新檢查以上步驟。
```
g++ -v
gcc -v
```

除了以上之外我還新增了include和library的變數，但大部分教學似乎沒怎麼說到，所以我不太確定是否必要。
![mingw-w64-install-10.JPG](https://raw.githubusercontent.com/alexmav04/alexmav04.github.io/master/_posts/computer/img/mingw-w64-install/mingw-w64-install-10.JPG)

![mingw-w64-install-11.JPG](https://raw.githubusercontent.com/alexmav04/alexmav04.github.io/master/_posts/computer/img/mingw-w64-install/mingw-w64-install-11.JPG)

到目前為止應該設置完成了。接下來是VScode的設置。

## VScode設置
這裡之後寫。

---
## 參考  
♦ [MinGW-w64安装教程——著名C/C++编译器GCC的Windows版本](https://zhuanlan.zhihu.com/p/76613134)  
♦ [window下g++' 不是内部或外部命令](https://blog.csdn.net/wzhwei1987/article/details/83414218)  
♦ [超簡單Visual Studio Code C/C++設定步驟](https://medium.com/@c52chungyuny/%E8%B6%85%E7%B0%A1%E5%96%AEvisual-studio-code-c-c-%E8%A8%AD%E5%AE%9A%E6%AD%A5%E9%A9%9F-a360be1487c)




