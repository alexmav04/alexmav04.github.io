---
title: "[Windows]包裝的程式不被系統所信任：談談簽章"
layout: single
author_profile: true
comments: true
categories:
  - computer
tags:
  - Windows
  - Code Signing
sidebar:
  nav: "sidepost"
---

其實此篇是延續上回[[Python]使用pyinstaller打包程式](https://alexmav04.github.io/computer/pyinstaller/)，打包上倒是還好，沒遇上太大困難點，網路上資源也很豐富，這次遇到比較大的困難反而是：在給別人使用時，才發現副檔名exe的這種執行檔，電腦超級敏感！

## 程式不被信任

其實也算是一種積極防禦啦，畢竟病毒到處都是，而這種執行檔只要有機會放進你的電腦，就有機會自己開始執行，甚至成長。第一道防線絕對是自己不要去下載來路不明的檔案，因此這種自己寫、沒有經過認證的（也就是現在很常看到的「未知的發行者」），定義上就叫做來路不明的檔案。因此很多雲端也會防堵這樣子的檔案，自己存OK；要給別人？你先緩緩。

但總有一些時候只是自己人要用，也沒打算幹大事，卻不能將寫好的工具給別人，實在是有夠麻煩。即使好好地弄好了載點，卻仍有下載後被偵測為病毒的可能性，要別人放心使用是真的蠻為難人家的。

這幾天查資料才發現，原來當初在發布程式中的「憑證」這東西不是沒有用的啊！所謂的憑證是指「程式碼簽章」或是「數位簽章」，基本上算是一種基本的證明，表示這個程式沒有被別人竄改過、並表明出品人或出品公司是誰等等。至於要確認軟體有沒有數位簽章，在 Windows 中可以對軟體點按**右鍵** → **內容**，並且會有個**數位簽章**的頁籤，如果有內容的話，也可以針對各個簽章查看資訊。

![code-signing-01](https://i.imgur.com/qRvPpNg.png)

But!!!! 這個簽章必須要向微軟認可的發行商購買，且畢竟是有「認證」的概念在裡面，因此申請流程並不簡單，通常無法以個人名義購買，而是需要以公司名義購買（就像上方軟體裡面會有的公司發行簽章）。

### 自然人憑證簽章

這是第一個方式，自然人憑證是由內政部憑證管理中心所簽發的憑證，許多網路業務都可以以這張憑證代表本人，概念有點像是「網路身分證」。其實蠻方便的，但不知道為什麼身邊申辦的比例很少。

有注意到上方一句話是重點嗎？「以這張憑證代表本人」，且這張憑證是經政府機關認證的，也受 Windows 認可，也就是說，我們可以以這張憑證下去製作簽章，那麼簽章產生出來後，上面的簽署人自然會是自己的名字。

簡單來說指令長這樣：
```
"signtool.exe的位置" sign /a /tr "憑證伺服器" 檔案位置
```

例如說我的指令長這樣：
```
"C:\Program Files (x86)\Windows Kits\10\bin\10.0.18362.0\x64\signtool.exe" sign /a /tr http://timestamp.sectigo.com "D:\tool.exe"
```

另外查到的指令大約是這樣：
```
"signtool.exe的位置" sign  /n "簽章名字" /i "內政部憑證管理中心" /t "http://timestamp.verisign.com/scripts/timstamp.dll"  /v D:\tool.exe
```
內政部憑證管理中心那個部分就是直接打「內政部憑證管理中心」，表示發憑證的機構。簽章名字代表是申請自然人憑證上面那個名字。其他部分差不多。

但我不是很確定是不是少了什麼步驟，這幾個我都沒有成功。如果有需要這樣方式簽署的話，可以參考這篇文章：[為 Pyinstaller 產生的程式簽章 – 使用自然人憑証](https://moon-half.info/p/3039)

### 另一個加上簽章之方式

一開始我選擇自然人憑證簽章，感覺應該要很容易進行的，但我操作時一直沒有出現自然人憑證的視窗（需要輸入PIN碼），導致似乎有成功產生簽章，卻不是我期待的樣子。但網路上關於這方面的資訊也相當陳舊，較近的資料已經找不到了，有點可惜，不然就可以看到完整憑證的樣子了。

但也沒關係，還有一個不太正式的方式：用 Window 的內建工具產生憑證。

首先需要確定有沒有相關工具：  
`where /R "C:\Program Files (x86)\Windows Kits" makecert.*`

若找不到，那可能要先下載 Windows Kits。

再來！！！我查到的資料只需要三行指令，先就我能夠理解的部分加以說明：

`"makecert工具位置" -a sha1 -b 憑證開始時間 -e 憑證結束時間 -cy authority -eku 1.3.6.1.5.5.7.3.3 -sv 憑證名.pvk -r -n "CN=簽署人名字, E=信箱" 憑證名.cer`

`"cert2spc工具位置" 憑證名.cer 憑證名.spc`

`"pvk2pfx工具位置" -pvk 憑證名.pvk -spc 憑證名.spc -po 1234(這啥) -pfx 憑證名.pfx -f`

因為我是在存放檔案的資料夾進行的，因此最前面使用工具都必須要加上確切的位置，系統才知道要去哪裡找這工具使用。其他沒特別說明的部分就是目前不太了解，若已經知道了會上來更新文章。

例如我的指令長這樣：
```
> "C:\Program Files (x86)\Windows Kits\10\bin\10.0.18362.0\x64\makecert" -a sha1 -b 01/01/2003 -e 12/31/2099 -cy authority -eku 1.3.6.1.5.5.7.3.3 -sv myCA.pvk -r -n "CN=my_name, E=mail@domain.com" myCA.cer
```

```
> "C:\Program Files (x86)\Windows Kits\10\bin\10.0.18362.0\x64\cert2spc" myCA.cer myCA.spc
```

```
> "C:\Program Files (x86)\Windows Kits\10\bin\10.0.18362.0\x64\pvk2pfx" -pvk myCA.pvk -spc myCA.spc -po 1234 -pfx myCA.pfx -f
```
過程中有可能出現此視窗，但這個密碼我不論有沒有設定，放在軟體上目前沒看到什麼差別。

![code-signing-02](https://i.imgur.com/RcAxcwX.png)

嘿對我連上面指令中的1234到底要幹嘛都不知道，但應急先都照教學的打上去，之後再慢慢看文件。執行完上方指令，應該會在指令執行位置出現幾個東西：

![code-signing-03](https://i.imgur.com/TQtCn98.png)

（其實應該還有一個檔案，但是沒有截到圖）

重要的是附檔名為`.pfx`的檔案，那個檔案是我們待會要附加在程式上面的東西（圖示看起來也一整個比較高級XD）。

接著要幫程式加上剛剛產生的簽章，共有兩行指令：

`"signtool.exe工具位置" sign /f 憑證名.pfx /p 1234 /tr 憑證伺服器 /v "要加上簽章的程式位置"`

`"signtool.exe工具位置" sign /fd sha256 /f 憑證名.pfx /p 1234 /as /tr 憑證伺服器 /v "要加上簽章的程式位置"`

我的指令長這樣：
```
"C:\Program Files (x86)\Windows Kits\10\bin\10.0.18362.0\x64\signtool.exe" sign /f myCA.pfx /p 1234 /tr http://timestamp.sectigo.com /v "D:\tool.exe"
```

```
"C:\Program Files (x86)\Windows Kits\10\bin\10.0.18362.0\x64\signtool.exe" sign /fd sha256 /f myCA.pfx /p 1234 /as /tr http://sha256timestamp.ws.symantec.com/sha256/timestamp /v "D:\tool.exe"
```

若執行指令後都是出現`Succeeded`或是`Successfully signed`表示成功將憑證附加上去了，但很可惜的是這種方式因為不是認證機構發的簽章，沒有經過認證的簽章，還是有被擋下來的可能性。

可以看到已經新增了一個憑證：

![code-signing-04](https://i.imgur.com/kCXfTHt.png)

查看詳細資料也可以更明白剛剛到底做了什麼事情：

![code-signing-05](https://i.imgur.com/B60luFU.png)

## 參考資料
這次在整理參考資料時發現，資料都好久以前的，蠻好奇為什麼近兩三年沒有相關文章。

* [[Windows] 幫自己的exe檔建立自己的數位簽章](http://limitx5.blogspot.com/2017/05/windows-exe.html)
* [為 Pyinstaller 產生的程式簽章 – 使用自然人憑証](https://moon-half.info/p/3039)
* [SignTool.exe (簽署工具)文件](https://docs.microsoft.com/zh-tw/dotnet/framework/tools/signtool-exe)
* [[Code Signing] 利用自然人憑證進行程式碼簽章](https://dotblogs.com.tw/Mystic_Pieces/2018/06/03/150349)
* [使用私人憑證加簽驅動程式](https://steward-fu.github.io/website/driver/wdm/self_sign.htm)