---
title: "[Ubuntu]錯誤訊息:unable to resolve host"
layout: single
author_profile: true
comments: true
categories:
  - computer
tags:
  - Linux
  - Ubuntu
  - system
  - error
sidebar:
  nav: "sidepost"
---
Linux系統給我的感覺就是必須為自己的所有行為負責（主要是一個錯誤就必須抽絲剝繭）。今天我只是要安裝個ruby環境就突然碰到不少問題，將問題做個分類一一記錄。

基本上這個問題根本不該發生，是自己蠢導致。因為有點久沒開另一台系統是Ubuntu的電腦，上次對它做了什麼事都忘了，結果`apt-get update`或是`apt-get upgrade`都有詭異的訊息：
```
sudo: unable to resolve host
```

完整訊息被我洗掉了，但大致上是長這樣。  
網路上蠻多文章都有寫到這個，要去改/etc/hosts文件：
```
sudo vim /etc/hosts
```
我用vim去修改，相信使用Linux系統的應該是不陌生。
在裡頭可以看到localhost的訊息：(以下是hosts文件內容)
```
127.0.0.1 localhost
127.0.0.1 alex-balabala
```

因為文章是教在第一條下面"加入"第二條文字，所以我看到我明明也有加了就沒再繼續研究，並沒有發現有任何問題。但實在是找不出原因，後來偶然瞄到我的磁碟名稱...媽呀！我不知道在什麼時候耍白癡把磁碟名稱改掉了！

例如我原先磁碟假設是alex-balabala，後來疑似在某個喝醉的夜晚我改成blex-hahaha隨便一個名字，加上我一陣子沒用這台電腦，完全不記得這回事。

那電腦當然找不到我的主機啊啊啊啊～  
後來我嘗試改改看：
```
127.0.0.1 localhost
127.0.0.1 blex-hahaha
```
就...好了。整個過程蠻無聊的，竟然也能寫成一篇文章，我感到羞愧，只能好好繼續學習了T-T。