---
title: "[Python]使用pyinstaller打包程式"
layout: single
author_profile: true
comments: true
categories:
  - computer
tags:
  - Python
  - Code Signing
sidebar:
  nav: "sidepost"
---

有時候做完了程式想將他打包起來，當成桌面應用程式或是直接在命令列呼叫，方便許多。這次要介紹的是打包Python程式成 Windows 中 exe 檔的工具－pyinstaller。

## 使用pyinstaller打包

指令蠻簡單的，基本上是：安裝pyinstaller → 使用 pyinstaller 打包。

安裝pyinstaller：
```
pip install pyinstaller
```

使用pyinstaller打包：
```
pyinstaller -F "file.py"
```

就這樣。咦？事情就這樣結束的話寫屁文章XD

## 發現檔案過大

Python 比較麻煩的地方是，如果沒有留意設定，包出來的檔案就換變超級大，因為預設會將整個用不到的環境也全包進去。如果要去除掉這些有的沒的東西，最簡單的方式是進入到乾淨的虛擬環境中打包：

### 使用虛擬環境

首先指定好環境，並且使用這個虛擬環境的shell：  
```
pipenv --python 3.7
pipenv shell
```

### 安裝套件

使用虛擬環境要注意的地方，請將環境視為一個什麼都沒有的「原始部落」。若在開發程式曾經安裝什麼套件、曾經對環境做過什麼設定，皆需要在這個虛擬環境中重新建置一遍。

接著在這個環境中安裝 pyinstaller，以及其他環境上的設置和更動：
```
pipenv install pyinstaller
pipenv install ...
```

### 最後使用 pyinstaller 打包
使用pyinstaller打包：
```
pyinstaller -F "file.py"
```

## 疑似未安裝PyQt5
打包時以為萬無一失（沒打算做任何的最佳化，單純想趕快了事），誰知道噴出許多看不是太懂得程序，最後一行硬生生給我寫：
`AttributeError: Module ‘PyQt5‘ has no attribute ‘__version__‘`

呃…… 好喔。  
當下很直接的想法是，當初認識Python不深時我裝了很肥的Anaconda，竟然還會缺東西嗎？且我都是用Tkinter，怎麼會需要安裝另一套PyQt5？

只好先依照錯誤訊息乖乖安裝試試看：

```
pip install PyQt5 --user
```

果然就好了呢……　但完全沒概念為什麼出現此錯誤訊息。另外若沒有包到的套件，不論是命令視窗或是程式錯誤訊息也可能會出現此文字，記得安裝即可。

## 後記
打包程式雖然只有四個字，但在今年做出獨立作品的路上，才知道這檔事學問這麼多。這次果然也碰到困難點，但和上次遇到關於打包桌面程式的問題不同，這次反而在包裝上沒甚麼大問題（可能是程式較為單純、比較不複雜的關係），反而是在給別人使用程式時花了一點心力，可再參考看看我的分享－[[Python]包裝的程式不被系統所信任：談談簽章](https://alexmav04.github.io/computer/code-signing/)

## 參考資料
* [Python程式包裝成 EXE 執行檔](https://ithelp.ithome.com.tw/articles/10226815)