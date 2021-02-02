---
title: "[jekyll]No such file or directory - git ls-files"
layout: single
author_profile: true
comments: true
categories:
  - computer
tags:
  - jekyll
  - Ruby
  - error
sidebar:
  nav: "sidepost"
---
快被氣死，不知道是巧合還是真的是 Windows 更新有問題，每次更新完一定有一個東西或環境需要除錯……

這次遇到的狀況是：

我寫完文章後習慣先用 jekyll 建置本機網站運作測試，看看我的文是否有正確地按照我想要的方式呈現。兩天前才用的沒啥問題。結果，昨天更新完 Windows 後今天突然噴出我完全沒看過的錯誤：

```
[!] There was an error parsing `Gemfile`:
[!] 這裡我忘了寫啥……
There was an error while loading : No such file or directory - git ls-files -z. Bundler cannot continue.
```

抱歉，難得遇上錯誤結果忘了截下珍貴畫面，簡而言之錯誤應該差不多長這樣。

後來嘗試了  
`bundle install`，失敗  
`bundle exec rake test`，失敗  
`gem install pygments.rb`，有跑東西  
`gem install bundler`，有跑東西  
`bundle install`，依然失敗  

看了看錯誤應該是有裝到 bundle 才會跑出無法運行的錯誤，所以我的方向可能是錯的。後來看到此篇 stackoverflow 的問題 [No such file or directory - git ls-files — WINDOWS](https://stackoverflow.com/questions/9793806/no-such-file-or-directory-git-ls-files-windows)，感覺和我遇到的狀況應該比較相近。

於是照著裡面給的方式跑去新增我的環境變數：
```
C:\Program Files\Git\bin
C:\Program Files\Git
```
完全是瞎子摸象。

因為還是跑不出啥鳥，接著將 ruby 的命令環境先關閉再重啟跑一次 `jekyll s`，有趣了，噴出另一種錯誤：

```
You have already activated public_suffix 4.0.6, but your Gemfile requires public_suffix 4.0.5. Prepending `bundle exec` to your command may solve this. (Gem::LoadError)
```

通常看到數字直覺就是版本錯誤，原本要開啟瘋狂 update 模式，但看見後面有個命令建議，想說先照著做做看。

`bundle exec rake`

噴出以下訊息：
```
rake aborted!
Don't know how to build task 'default' (See the list of available tasks with `rake --tasks`)
```
總麼覺得我被嗆了……　這個環境真的很不熟悉。  
不知哪來的想法，突然想試試 `bundle update rake`，結果還真有運行一些東西（欸不是，跟建議的不一樣啊）。

好吧，既然有運行了一些東西，那應該是成功了嗎？又隨便打了 `jekyll s` 測試，果然還是失敗！因此回到最初，更新 bundle 呢？

`bundle update`

神奇的是跟剛剛`bundle update rake`時跑出的進行的動作明明完全一模一樣，但再次執行`jekyll s`時，卻成功建置了！

完全不懂為什麼會成功……

因為我剛剛有關閉過命令視窗，因此只能就這個視窗中最近的修正來做測試，也就是一開始新增的環境變數將它刪除。想說若有錯誤，那應該就是因為環境變數的問題了，結果……

即使刪除了還是可以跑啊！！！！！

那到底是怎麼回事……

## 參考資料
* [No such file or directory - git ls-files — WINDOWS](https://stackoverflow.com/questions/9793806/no-such-file-or-directory-git-ls-files-windows)