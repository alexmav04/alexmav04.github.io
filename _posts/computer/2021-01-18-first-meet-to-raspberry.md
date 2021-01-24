---
title: "[Pi]無頭式安裝Raspbian"
layout: single
author_profile: true
comments: true
categories:
  - computer
tags:
  - raspberry pi
sidebar:
  nav: "sidepost"
---
本次使用無頭式安裝法將 Raspbian 裝進 Raspberry Pi 4。
要注意這裡不是用NOOBS裝，這個方式是為了只有一塊板子，沒有額外的螢幕以及鍵盤的人用的。

順序大概是這樣：
**格式化硬碟->作業系統安裝->加入wifi和ssh檔（設定檔）**
前置作業完成後，啟動Raspberry Pi，這些檔案都會被自動讀入。  
最後**使用SSH遠端連線**至這塊小主機。

## 格式化硬碟
所有動作之前，要先做的是格式化自己的microSD card（也許會需要一個讀卡機）。  
這裡就用SD Card Formatter這個工具幫這張卡格式化，完成後格式應該會是FAT。

## 下載作業系統
去樹莓派的網站中下載作業系統，我初次使用的是Raspbian。

之前參考很多網站文章，都會教如何燒錄映像檔。不過隨著時間演進，我發現Raspberry Pi的網站似乎提供了更進階好用的東西：
Raspberry Pi Imager for XXX

![first-meet-to-raspberry-01](https://i.imgur.com/F87p3xd.jpg)

這個就我使用上的理解，似乎就是把寫入的動作以及Raspberry會用到的作業系統都包起來。以Windows來說，我就不需要再跑去其他地方下載一個Win32DiskImager。

![first-meet-to-raspberry-02](https://i.imgur.com/mG3bFMA.jpg)

簡潔明瞭，一個選項挑作業系統、另一個選項挑位置，接著寫入。

當然，若是已經指定自己要的作業系統，也是能夠用平時我們常用的方式去裝作業系統。這個工具對於初學者來講算蠻親民的（尤其是對沒碰過Linux系統的來說）。

## 設定檔
可能是非相關科系的關係，這個步驟我覺得蠻特別的。就是加一些檔案，有的甚至沒有副檔名和內容，原來可以進行相關設定。

###  檢查自己網路的加密類型
因為是用wifi連線，我要查我現在使用的網路的資訊。總而言之（以Win10來說）就是在控制台中找到「網路和網際網路」，點擊自己的網路連線應該能夠確認，這裡就不再贅述。

###  加入wifi設定檔

對於不同網路連線型態有不同的設置方式，這些設置方式在網路上有很多資源。我的這種應該算是最普通且常見的。

這個檔案我用NotePad++編輯。
（因為Unix和Windows的文件格式不一樣，例如換行符號是`\n`，而在Window上會是`\r\n`，所以必須使用Linux系統接受的編輯器去編輯）

檔名：wpa_supplicant.conf
內容：
```
    country=GB
    ctrl_interface=DIR=/var/run/wpa_supplicant GROUP=netdev
    update_config=1

    network={
        ssid="SSID_NAME" （這裡要填入連線名字）
        psk="SSID_KEY"　（這裡填入密碼）
        key_mgmt=WPA-PSK
        scan_ssid=1
    } 
```

編輯完後直接加入硬碟中。

###  加入ssh
因為安全因素，Raspberry Pi最初的設定是不能用ssh的。但想當然爾一般用戶如我們怎麼可能不使用XD
這裡很簡單，就直接加個叫做"ssh"的檔案就好了（連副檔名都不需要有的空白檔）

因此在 boot 資料夾中（也就是樹莓派的硬碟中），除了剛剛安裝的系統外，還會多兩個我們自己加進去的檔案：

![first-meet-to-raspberry-03](https://i.imgur.com/8Oam6nJ.jpg)

## 首次啟動樹莓派
就...先插電XD  

預設帳號為：pi  
預設密碼為：raspberry

喔對了，要把microSD卡，也就是你的作業系統裝進去，再插電。至於燈號可參考看看 [Raspberry Pi學習筆記（十五）：樹莓派LED指示燈說明](https://yanwei-liu.medium.com/raspberry-pi%E5%AD%B8%E7%BF%92%E7%AD%86%E8%A8%98-%E5%8D%81%E4%BA%94-%E6%A8%B9%E8%8E%93%E6%B4%BEled%E6%8C%87%E7%A4%BA%E7%87%88%E8%AA%AA%E6%98%8E-9f8caf9a244c)，這個可以幫助自己做初步判斷。

怎麼查中文很多都中國文章... 看來在台灣即便很多人玩卻很少交流。

### 找尋連線資料
正常來說機器應該會自動連線到 wifi 了。
首先要知道自己的ip位置，在自己的電腦打開命令視窗輸入
```
ipconfig
```
接著會出現各種關於網路的資訊。因為我用的是wifi，所以對到無線網路那個部分。

![first-meet-to-raspberry-04](https://i.imgur.com/ibhyvvC.jpg)

用同樣的網路 ip 位置應該不會差太多，接著使用 Advanced IP Scanner 這個工具去搜尋機器的位置。

![first-meet-to-raspberry-05](https://i.imgur.com/2EimvFC.jpg)

看起來是 66 結尾的，我使用的是 PuTTy 這個連線工具來執行我的遠端連線。

### 終端機遠端連線
PuTTy 的使用方式非常簡單，
![first-meet-to-raspberry-06](https://i.imgur.com/xPRMcMS.jpg)

打上剛查到的 IP 以及選擇 SSH 即可儲存。想要修改先前的 Session 只要按一下名字後按 Load 即可進行修改。 

### 畫面遠端連線
如果要想要圖形畫面，可以試試另一款更全能的工具：MobaXterm。
除了終端機畫面以外，也有桌面選項。首先我們要加入自己的連線，按下 New Session 新增：

![first-meet-to-raspberry-07](https://i.imgur.com/BJRgESb.jpg)

最上方選擇 SSH 連線後按照以下設置：
* Remote host - 填寫連線 IP
* Specify user name - 自己命名的連線名字
* Remote environment - 選擇 LXDE desktop

![first-meet-to-raspberry-08](https://i.imgur.com/qWeBtf5.jpg)

新增好自己的 Session 後，以後要連線就是按右鍵，點選 Connect as... 並輸入用戶名稱及密碼即可。

![first-meet-to-raspberry-09](https://i.imgur.com/HMB9q42.jpg)

當然第一次登入 raspberry pi 會強烈建議你換一組新密碼，畢竟預設的不是太安全。

---
## 參考資料
♦ [Raspberry Pi | 樹莓派安裝起手式：無頭式安裝法](https://hugheschung.blogspot.com/2019/01/raspberry-pi.html)
