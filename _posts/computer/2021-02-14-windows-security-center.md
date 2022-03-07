---
title: "[Windows]Security Center以及病毒防護無法開啟（Win10）"
layout: single
author_profile: true
comments: true
categories:
  - computer
tags:
  - Windows
  - Win10
  - trouble shooting
sidebar:
  nav: "sidepost"
---
這篇文章的緣由是電腦上 Windows 的安全性頁面呈現空白，但又很不想裝第三方防毒軟體。

至於為什麼會被弄成完全無法使用嗎…… 我這台是二手的，原本就有裝 Windows ，似乎是某公家機關或公司退役下來的電腦，在猜想可能是為了不要讓 Windows Defender 影響到自己公司的防毒。那麼其他更深入的細節（或許是KMS）和原因就自行思考吧XD

## 開啟 Windows Defender

頁面空白的原因比較好修正，很明顯是相關項目被關閉。因此現在要做的是開啟 Windows Defender。

### 確認有無安裝第三方防毒軟體

所謂的第三方防毒軟體，指的就是任何不屬於 Windows 內建的防禦軟體。通常有第三方軟體，Windows Defender 會自動關閉，即使用再多方法打開都沒用，因此首要之務就是確認有無第三方防毒軟體。

有時候電腦使用久了確實會忘記是否有安裝（我是覺得防毒軟體的煩躁度應該是不會被忘記啦 XD），這時候可以點工具列上的搜尋，打上「新增或移除程式」，或是點選「設定」（在電源上方）→「應用程式」也可以，在眾多程式列表中查看有無防毒軟體。或是可以看看自己工具列的右下角，因為防毒軟體很煩，會時不時刷存在感，蠻常出現在螢幕右下角提醒你要掃毒、丟檔案等等的。

### 啟用 Windows Defender Service

1. 先叫出「執行視窗」：按 **Win視窗鍵** + **R**，或是對著開始鍵按右鍵，點選選單中的執行。
2. 接著在方框中輸入 services.msc 後按確定。
3.  Windows Defender Service ，按右鍵進行啟用。

### 無法打開 defender 的插曲

我這邊兩台電腦做這動作時，發現其中一台會有打不開的狀況，出現啟動後又被停止的警告視窗。建議大家試試這個方法，看看有沒有毀損檔案。

打開命令視窗（ **Win視窗鍵** + **R**，輸入 **cmd**），依序輸入以下指令。　

```
Dism /Online /Cleanup-Image /CheckHealth
Dism /Online /Cleanup-Image /ScanHealth
Dism /Online /Cleanup-Image /RestoreHealth

sfc /scannow
```

若還是無法，可以使用看看這個方法：  
用 Notepad++ 或是記事本，新增以下檔案。

檔名：.reg
```
Windows Registry Editor Version 5.00

[HKEY_LOCAL_MACHINE\SOFTWARE\Policies\Microsoft\Windows Defender]

"DisableAntiSpyware"=dword:00000000

[HKEY_LOCAL_MACHINE\SOFTWARE\Policies\Microsoft\Windows Defender\Real-Time Protection]

"DisableBehaviorMonitoring"=dword:00000000

"DisableIOAVProtection"=dword:00000000

"DisableOnAccessProtection"=dword:00000000

"DisableRealtimeMonitoring"=dword:00000000

[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\SecurityHealthService]

"Start"=dword:00000002
```
存好檔後直接點這檔案兩下，會直接執行相關動作，接著再重新開機，回到前面步驟看有無開啟 Windows Defender。

仔細看上面檔案內容，很容易看得出來就是更改「登錄編輯程式」而已。如果想自己手動更改，**Win視窗鍵** + **R**，輸入 **regedit**，接著再依照以上路徑，找到相關的參數修改即可。

### 插曲中的插曲

也是其中一台電腦有這個問題，執行後出現錯誤577的警告視窗。解決方式參考以下：

打開命令視窗（ **Win視窗鍵** + **R**，輸入 **cmd**），貼上以下命令：

```
reg add "HKEY_LOCAL_MACHINE\SOFTWARE\Policies\Microsoft\Windows Defender" /v "DisableAntiSpyware" /d 0 /t REG_DWORD /f
```
接下來再回去前面步驟打開 Windows Defender 就可以了。

## 開啟安全中心

以上動作都不是太難理解，安全性視窗也終於能夠順利開啟了。但當我想要掃描電腦時，發現病毒防護資訊始終處在更新中（也就是畫面顯示的「正在取得保護資訊」），前後大概過了八小時仍然沒有更新成功的資訊，因此也無法使用掃描功能。

![windows-security-center-01](https://i.imgur.com/VxIwqif.jpg)

無意間發現一個地方：點選「本機」，按右鍵選單選擇「內容」後，點選右下角「安全性與維護」。

![windows-security-center-02](https://i.imgur.com/vJJwOZB.jpg)

原來我的安全中心是關閉的狀態啊！依照常理推測，或許問題源頭就是出在安全性中心未開啟，所以病毒定義檔之類的更新才會一直無法完成。

![windows-security-center-03](https://i.imgur.com/cTS335n.jpg)

因此理所當然地點選了立即開啟，不過事情沒有想像中那麼簡單。

![windows-security-center-04](https://i.imgur.com/oOxSPDq.jpg)

媽的。

自然聯想到最前頭開啟 Windows Defender 的方法：

1. 先叫出「執行視窗」：按 **Win視窗鍵** + **R**，或是對著開始鍵按右鍵，點選選單中的執行。
2. 接著在方框中輸入 services.msc 後按確定。
3. 找到 Security Center 後，按兩下點選啟用

不過到第三步驟就卡住了…… 整個按鈕和下拉選單都是鎖定住的，完全沒有能夠更改的選項。如果能夠打開，那就直接選自動並啟用即可，若和我一樣打不開，就試試以下方法吧！

**Win視窗鍵** + **R**，輸入 **regedit**。猜得沒錯，我們需要更改機碼。
依照以下路徑：
```
HKEY_LOCAL_MACHINE\SOFTWARE\SYSTEM\CurrentControlSet\Services\wscsvc
```
找到名稱為 Start 的參數選項，我原本的數值是 4。

![windows-security-center-05](https://i.imgur.com/nwKUoJF.jpg)

滑鼠點兩下進去編輯，將數值資料改為2後點確定。

![windows-security-center-07](https://i.imgur.com/MP9flRX.jpg)


重新開機後，回到前面開啟 services.msc 的部分，可以看到已經能夠點選啟用了。之後前往先前我們去的安全性與維護視窗（點選「本機」，按右鍵選單選擇「內容」後，點選右下角「安全性與維護」），確認是否已開啟。

以上步驟皆完成後，我們可以回到 Windows Defender 查看病毒與威脅防護有沒有成功更新完成。

![windows-security-center-08](https://i.imgur.com/tRVgvnW.jpg)

喔終於看起來不太一樣，開啟 Windows 安全性，可以看到已經能夠掃描電腦，也有一些設置能夠點選了！

![windows-security-center-10](https://i.imgur.com/sWpu9p9.jpg)

## 參考資料
* [更新Win10后Windows defender无法打开，提示"错误577"](https://answers.microsoft.com/zh-hans/protect/forum/protect_defender-protect_start-windows_other/%E6%9B%B4%E6%96%B0win10%E5%90%8Ewindows/f4196340-5606-4c5f-b587-383d86e849a9?page=2)
* [Getting an error 577 when I turn Windows defender from Manual to Automatic](https://answers.microsoft.com/en-us/protect/forum/protect_defender-protect_scanning-windows_8/getting-an-error-577-when-i-turn-windows-defender/fb2d4a5b-c578-4217-9f02-f30d28e2ca24)
* [Windows 8 Ent. 中的Windows Defender 顯示已關閉要如何開啟](https://answers.microsoft.com/zh-hant/windows/forum/windows_8-security/windows-8-ent-%E4%B8%AD%E7%9A%84windows-defender/c0273361-0747-4767-ab51-52a9f99401dc?msgId=6c11cc7a-f1ad-435f-8b4e-3c64341037a5)
* [windows defender无法正常使用(病毒威胁防护和防火墙网络防护两个模块)](https://answers.microsoft.com/zh-hans/protect/forum/protect_defender-protect_updating-windows_10/windows/72ed7685-62d3-41a1-a3ea-0a39b11a9022)