---
title: "[jekyll]錯誤訊息Could not find gem..."
layout: single
author_profile: true
comments: true
categories:
  - computer
tags:
  - Ubuntu
  - jekyll
  - trouble shooting
sidebar:
  nav: "sidepost"
---
最近在Ubuntu上安裝jekyll，遇到不少問題，好險有各種強大的網友們。直到今天真正要使用jekyll去生成我的網站時，各種錯誤訊息來襲，只好認命繼續看哪裡出錯了...

錯誤內容：
```
Could not find gem 'jekyll-paginate (~>1.1)', which is required by gem 'balabala'
Could not find gem 'jekyll-sitemap (~>1.3)', .....（後面都差不多）
```
看起來應該是安裝jekyll漏了什麼東西我沒發現，當時為了除掉其他的bug已經太混亂。
根據[jekyll issue](https://github.com/jekyll/jekyll/issues/4972)這篇我使用以下命令：

```
sudo gem install pygments.rb
gem install bundler
bundle install
```
執行完就正常了。

## 後記
之後有查一下pygments.rb和bundler到底是什麼東西，bundler似乎是另一個套件管理工具？