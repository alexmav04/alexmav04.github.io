---
title: "[Ubuntu]使用RVM安裝Ruby"
layout: single
author_profile: true
comments: true
categories:
  - computer
tags:
  - Ubuntu
  - system
  - error
sidebar:
  nav: "sidepost"
---
首先必須講，盡量不要用 `sudo apt-get install ruby` 這個指令安裝。有在Ruby社群的可能也有被建議過不要使用套件管理工具來安裝，因為這個會安裝一個穩定但是老舊的版本（好像是2.3.?）。而我是為了jekyll而安裝，那時它就有跑出錯誤訊息說Ruby需要2.4.0的版本。所以我整個砍掉重練...  
（可惜錯誤訊息被我洗掉了，不然可以給往後的我留作紀念）

既然不用套件管理工具，那該拿什麼來安裝？根據網路上大神們的說法：「若系統或套件管理員提供的 Ruby 版本過時的話，可以使用第三方的安裝工具來安裝。」因為我玩這個系統還不夠久，一知半解的狀態下直接選擇RVM，這個有內建版本控制。

## 開始安裝套件
### 安裝RVM
[官網](https://rvm.io/)是寫這樣
```
gpg2 --recv-keys 409B6B1796C275462A1703113804BB82D39DC0E3 7D2BAF1CF37B13E2069D6956105BD0E739499BDB
\curl -sSL https://get.rvm.io | bash -s stable
\curl -sSL https://get.rvm.io | bash -s stable --rails
```

我是誤打誤撞找到[RVM package for Ubuntu](https://github.com/rvm/ubuntu_rvm)

```
sudo apt-add-repository -y ppa:rael-gc/rvm
sudo apt-get update
sudo apt-get install rvm
```

### 更改終端機視窗
不太確定這個的用意，先記錄。
```
echo 'source "/etc/profile.d/rvm.sh"' >> ~/.bashrc
```

### 重啟系統
這裡超級重要，那時看教學只有字，沒有圖沒有指令，就下意識跳過這個步驟，連reboot關鍵字都沒看到。  
關於原因，文件是這樣說的：
> scripts that needs to be reloaded, you're now member of rvm group

總而言之就像有的東西更新完了會要求重新啟動那樣，這個也是，而且沒有"下個世紀"的選項XD  
我那時忽略的結果，就是不管怎麼輸入 `rvm install ruby` ，它總是會出現錯誤訊息：

```
There has been an error fetching the ruby interpreter. Halting the installation.
```
有趣的是，好像蠻多人和我一樣直接忽略這個步驟XD

喔對了，這個筆記不是即時記錄，有些事情發生順序有點搞混了。  
看草稿有記錄到這個錯誤訊息：
```
rvm command not found
```

用了以下的方式解決：  
(if using login-shell)
```
echo "source $HOME/.rvm/scripts/rvm" >> ~/.bash_profile
```
(if using non-login shell)
```
echo "source $HOME/.rvm/scripts/rvm" >> ~/.bashrc
```
這兩個問題不太確定誰先誰後，那時還真是一片混亂= ="

### 安裝Ruby
也被前面問題搞得差不多了，開始安裝Ruby。
```
rvm install ruby
```
終於正常了，有夠感動。

### 安裝jekyll
先前提過我這次的目的其實是為了jekyll。  
首先要安裝開發工具：
```
sudo apt-get install ruby-full build-essential zlib1g-dev
```

設置一個gem安裝目錄，並配置環境變數：  
（說是要避免以 root 身份安裝Gems，這裡先記錄，之後看。）
```
echo ‘# Install Ruby Gems to ~/gems’ >> ~/.bashrc
echo ‘export GEM_HOME=“$HOME/gems”’ >> ~/.bashrc
echo ‘export PATH=“$HOME/gems/bin:$PATH”’ >> ~/.bashrc
source ~/.bashrc
```

最後就是jekyll了：
```
gem install jekyll bundler
```

## 後記
我比較驚訝的是原以為在Ubuntu上安裝會很順利，但接連幾個錯誤反而讓整個流程比在Window上操作還要耗時。那時光是忙著尋找解決方式，有很多小細節似乎沒記錄到T-T  
終歸結論應該是我對Linux系統還沒熟悉到可以隨心所欲...  
繼續加把勁學習吧！

---

## 參考：
♦ [為你自己學 Ruby on Rails](https://railsbook.tw/chapters/02-environment-setup.htmlv)  
♦ [安裝 Ruby](https://www.ruby-lang.org/zh_tw/documentation/installation/#rvm)  
♦ [ubuntu_rvm issue](https://github.com/rvm/ubuntu_rvm/issues/47)  
♦ [stackoverflow-rvm command not found](https://stackoverflow.com/questions/19595974/rvm-command-not-found)  
♦ [Ubuntu 18.04 安裝 Jekyll](https://matters.news/@kaixdev/ubuntu-18-04-%E5%AE%89%E8%A3%9D-jekyll-bafyreiakfcusg4cg2ljgmylawk4tdcuhpbczs72xugg77p2pvwy3nqlica)  
♦ [RVM package for Ubuntu](https://github.com/rvm/ubuntu_rvm)

