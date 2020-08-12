---
title: "[calculus] (ε, δ)-definition of limit"
layout: single
author_profile: true
comments: true
categories:
  - math
tags:
  - definition
  - calculus
  - 高淑蓉微積分
---
剛接觸微積分時，以為微積分不過就是像高中一樣，只是把算式多加幾個看起來很專業的符號。讀了數學才知道，才剛進門就碰觸到的 epsilon-delta 定義竟然是新手大魔王，一旦觀念不清楚後面再怎麼會算大概也不是真正了解微積分的真理，甚至往後到高階數學都會有一層霧在腦中。

以前我也是以為這種定義鑽牛角尖就跳過……　後果蠻慘的啦，很多分析概念都不懂或是自以為懂。反而是畢業了之後加了相關數學社團，裡面有很上進的高中生想要先了解微積分問了大家推薦的課程，看到下面留言一堆人二話不說推薦高淑蓉老師。聽了真的後悔……　後悔自己竟然觀望那麼久才聽XD　老師觀念超級清晰，前幾堂課也苦口婆心教大家做筆記的態度，非常受用也獲益良多。因此很想把他的觀念用我的方式再整理一次加深印象。這個課程非常適合初學者認識概念，也非常適合高階者再次釐清觀念。

廢話不多提，這篇主要是紀錄 \\(\varepsilon - \delta \\) definition 的基本觀念釐清。

---
## 基本前導概念
### 函式
首先要弄懂函式的概念，主要有幾點：  
* 一個定義域對應到一個值域的方式
* 不能一對多

### 極限的粗略概念
現在有一個函式\\(f(x)\\)，當 \\(x\\) 很靠近 \\(c\\) 時，這個函式會發生什麼事？  
我們會把這個狀況記錄成 \\(\displaystyle{\lim_{x\to c}f(x)}\\)

極限有幾項特點：  
* 極限討論的是"很靠近"某個點（例如點c好了）所對應的值，所以函數在那個點c有沒有定義並不重要。
* 從左邊靠近、從右邊靠近都會對應到同一個點，我們才會說這個極限存在；  
  換句話說，左極限、右極限存在且左極限=右極限，那麼這個函式的極限就存在。
* 我們討論的範圍是專注在某個點附近，也就是說我們只在意這範圍，再更外面的值，就算函式再怎麼亂跑，都不關我們的事。

### 極限記錄方式
當 \\(x\\) 很靠近 \\(2\\) 時，\\(f(x)=x^2\\)這個函式會越來越靠近\\(4\\)  
我們習慣記錄成：\\(\displaystyle{\lim_{x\to 2}f(x)=4}\\)

## 用符號記下來
我們討論的是數學，自然要把這些概念用數學語言表示出來。所以接下來就會慢慢帶入 \\(\varepsilon \\) 和 \\(\delta \\) 了

### 所謂很靠近的意思
上面對於極限的特點有講到"很靠近"，這個"很靠近"是指距離上的，在數學上我們通常習慣用絕對值去表示一個距離。這裡有兩種狀況：
* 在y軸上，我們用 \\(\varepsilon \\) 去靠近 \\(f(x)\\) 在y軸上的值 \\(L\\)  
  \\(\left| f(x)-L \right|<\varepsilon\\)
* 在x軸上，我們用 \\(\delta \\) 去靠近 \\(x\\) 在x軸上的值 \\(c\\)  
  \\(\left| x-c \right|<\delta\\)

老師在這邊有特別提醒我們注意這兩點的先後和對應關係，並說：  
對 \\(\varepsilon \\) 的靠近是由 \\(\delta \\) 的靠近所完成。

### 無敵靠近
既然都說要討論"極限"，也就是說我們的"很靠近"必須是無敵靠近，用老師的話說就是「要多靠近就有多靠近」。我們已經知道在x軸和y軸上變數的距離表示了，那麼對於這個無敵靠近的概念，又要如何用數學式子表示？

首先得先知道這個概念是動態的，因此數字並不會固定。

從上一個討論靠近這個意義的概念再次疊加上去：  
對於每一個 \\(\varepsilon \\) 靠近，皆存在有 \\(\delta \\) 的靠近，  
使得對每一個 \\(x\\) 在 \\(0< \left| x-c \right| <\delta\\) ，都滿足 \\(\left|f(x)-L\right|<\varepsilon\\)

## \\(\varepsilon - \delta \\) definition
接著把以上疊加過後的概念再用更數學的方式寫出來即可。

### Def
We say that \\(\displaystyle{\lim_{x \to c}f(x)=L}\\)  
if \\(\forall \varepsilon > 0, \exists \delta > 0\\)
such that \\(\forall x \; in \; 0<\left|x-c\right|<\delta, \; \left|f(x)-c\right|<\varepsilon\\)

### Def (從左靠近)
We say that \\(\displaystyle{\lim_{x \to c^-}f(x)=L}\\)  
if \\(\forall \varepsilon > 0, \exists \delta > 0\\)
such that \\(\forall x \; in \; (c-\delta,c), \; \left|f(x)-c\right|<\varepsilon\\)

### Def (從右靠近)
We say that \\(\displaystyle{\lim_{x \to c^+}f(x)=L}\\)  
if \\(\forall \varepsilon > 0, \exists \delta > 0\\)
such that \\(\forall x \; in \; (c,c+\delta), \; \left|f(x)-c\right|<\varepsilon\\)

### 要注意的點
#1  
為什麼是"先給 \\(\varepsilon > 0\\)"，而後才有"存在有\\(\delta > 0\\)"；  
而不是"先給\\(\delta > 0\\)"，而後才有"存在有\\(\varepsilon > 0\\)"？

要釐清這個，就看往前面筆記，關於 \\(\varepsilon\\) 和 \\(\delta\\) 的關係。

若給定了 \\(\delta > 0\\)，那就是給定了 x 的範圍 \\((c-\delta , c+\delta)\\)。既然給定了 x 的範圍，那 y 的範圍也定下來了，也就沒有討論"靠不靠近"的問題。

#2  
由先前的定義應該可以得知 \\(\delta\\) 和 \\(\varepsilon\\) 緊緊相依。而 \\(\delta\\) 會隨著 \\(\varepsilon\\) 變小（也就是 \\(\delta\\) 是隨著 \\(\varepsilon\\) 而決定），因此我們有時會為了強調這個關係，而將 \\(\delta\\) 寫成 \\(\delta (\varepsilon)\\) 。

#3  
\\(\varepsilon\\) 的存在並不唯一。只要取比 \\(\varepsilon\\) 小的都可以。（比它大的不知道走不走得進去，所以不行）

---

## 後記
以上筆記是聽高淑蓉老師的課後，稍微整理直接打出來的。往後應該會有越來越多這種筆記，藉此加深對觀念釐清的印象。

---

## 參考  
♦ [高淑蓉老師微積分（一）](https://www.youtube.com/watch?v=7qMu9OejpT8&list=PLS0SUwlYe8czw04JGine76IzoHc1MM8bO&index=8) 