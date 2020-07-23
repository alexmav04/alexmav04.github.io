---
title: "[numbers]阿基米德公理Archimedean axiom"
layout: single
author_profile: true
comments: true
categories:
  - math
tags:
  - Archimedean axiom
  - axiom
  - numbers
---
此為讀《數學分析基礎》之筆記。

## Axiom (Archimedean axiom)
敘述如下：  
\\(let \; a>0,b>0, then \; \exists N \in \mathbb{N}\ such\; that \; Na>b\\)

有任意兩個數（大於0），不論這兩個數是什麼，總是有個N能夠足夠大到讓其中一個數超越另一個數。  
例如說地板和毛毛雨，只要有足夠的時間，地板也能夠被雨所覆蓋（被超過）。

## 衍生例子和應用
* 龜兔賽跑  
  假設兔子以非常快的速度跑了一段非常長的距離，然後兔子開始睡覺。烏龜就算每天只能走1mm，最終還是能夠超越兔子。

* 推論  
  \\(\forall r>0, r\in \mathbb{R}, \exists N\in\mathbb{N}\ such\;that\;N>r\\)  
  白話講就是對任一個正實數，總是存在一個自然數會大於這個正實數。

  pf:  
  利用 \\(1>0\\) 和阿基米德公理，\\(\exists N\in\mathbb{N}\ such\;that\;N1=N>r\\)