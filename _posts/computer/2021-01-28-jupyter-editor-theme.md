---
title: "[Jupyter]更換佈景主題"
layout: single
author_profile: true
comments: true
categories:
  - computer
tags:
  - Jupyter
  - Python
sidebar:
  nav: "sidepost"
---
通常大家在學習平台甚至自己平常用的 Jupyter 都是預設的白色背景（印象中 MATLAB 好像也是如此），看久了眼睛實在是很累。

有鑑於最近從自己的手機預設主題到電腦的環境建置甚至到自己架設的網站都已經以較暗的主題為主了，Jupyter 再死撐著既定印象不換實在是說不過去，於是在此紀錄換主題的過程。

## 安裝

首先打開命令視窗，輸入以下指令安裝主題：
`pip install jupyter-themes`

並且更新到最新版本：
`pip install --upgrade jupyterthemes`

## 套用

接下來我會用到的參數有下面幾項，其實還有很多參數能使用，可參考[這裡的文件](https://github.com/dunovank/jupyter-themes#description-of-command-line-options)，因為我的要求沒很多，就先設定最簡單的幾項即可。

| 功能               | 參數   |
| -----------------  |:----- |
| 列出所有的主題       | -l     |
| 欲安裝的主題         | -t    |
| 程式碼的字型         | -f    |
| 程式碼的字體大小      | -fs   |
| 工具列顯示           | -T    |
| 檔名以及 Logo 顯示   | -N     |

套用的方式相當簡單，在命令視窗中輸入 `jt` 後打上參數以及選項即可。

### 列舉主題

例如以最需要改的主題為例：

首先我們需要看有哪些主題能夠修改：
`jt -l`

可看見有些主題可挑選：
```
Available Themes:
   chesterish
   grade3
   gruvboxd
   gruvboxl
   monokai
   oceans16
   onedork
   solarizedd
   solarizedl
```

### 更換主題

建議都試試看找到最適合自己的主題，輸入完指令後重新整理 Jupyter 視窗就可以立即看到了。

`jt -t 想要選擇的主題`

例如說我想試試 gruvboxd

`jt -t gruvboxd`

其他那些參數也可以一併加到後面，例如我想讓字體變為14、並且開啟工具箱和檔名及 Logo（因為改完主題後似乎會預設隱藏）。這種需求可以這樣輸入：

`jt -t oceans16 -fs 14 -N -T`

## 參考
* [jupyterthemes](https://github.com/dunovank/jupyter-themes)