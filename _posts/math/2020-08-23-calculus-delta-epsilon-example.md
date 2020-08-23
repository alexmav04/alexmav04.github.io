---
title: "[calculus] (ε, δ)-definition 題目範例"
layout: single
author_profile: true
comments: true
categories:
  - math
tags:
  - definition
  - calculus
  - 高淑蓉微積分
  - example
---
延續上一篇： [(ε, δ)-definition of limit](https://alexmav04.github.io/math/calculus-limit/) 

## 「存在有δ >0」的兩種狀況
在高淑蓉老師的微積分開放式課程中提到，「存在有 \\(\delta >0\\)」可分為兩種狀況：
1. 去找 \\(\delta >0\\) -> 「去證明 \\(\displaystyle{\lim_{x \to c}f(x)=L}\\)」  
   也就是說，定義尚未成立，因此要去找 \\(\delta >0\\)
2. 得到 \\(\delta >0\\) -> 「已知有 \\(\displaystyle{\lim_{x \to c}f(x)=L}\\)」  
   這種狀況是定義已經成立了（得到 \\(\delta >0\\)）

## 題目講解
### 證明limit-基本題型
e.g. Show that  \\(\displaystyle{\lim_{x \to 2}3x=6}\\)

proof:  
先給定 \\(\varepsilon\\)：Let \\(\varepsilon>0\\)  
再來要找 \\(x\\) 滿足 \\(\left | f(x)-L \right | < \varepsilon\\) 這個不等式，在這個例子就是要滿足 \\(\left | 3x-6 \right | < \varepsilon\\) 。

把上述式子化簡：  
\\(\left | 3x-6 \right | < \varepsilon\\)   
\\(\Rightarrow 3\left | x-2 \right | < \varepsilon\\)  
\\(\Rightarrow \left | x-2 \right | < \displaystyle{\frac{\varepsilon}{3}}\\)

有把定義搞懂的話，很容易就可以看出 \\(\displaystyle{\frac{\varepsilon}{3}}\\) 代表的意義是題目這個狀態所能取到的最大區間（也如之前所說，取的方式很多，只要比這個值還小就行）。

既然已經求出可以取到的最大區間了，我們可以直接把這個 \\(\displaystyle{\frac{\varepsilon}{3}}\\) 叫做 \\(\delta\\)，並把最後結論用數學語言寫完整一些：

Take \\(\delta = \displaystyle{\frac{\varepsilon}{3}}\\).  
Then \\(\forall x \; in \; 0 < \left | x-2 \right | < \delta, \; \left | 3x-6 \right | < \varepsilon \\)  
（這部分就湊出 (ε, δ)-definition 的敘述了）

最後要記得提到當初要證明的東西：  
Therefore \\(\displaystyle{\lim_{x \to 2}3x=6}\\)

### 證明limit-稍微變化的題型
e.g. Show that  \\(\displaystyle{\lim_{x \to 2}x^2=4}\\)

proof:  
題型一樣，只是過程稍微變化。
一樣先給定 \\(\varepsilon\\)：Let \\(\varepsilon>0\\)  
再來要找 \\(x\\) 滿足 \\(\left | x^2-4 \right | < \varepsilon\\) 這個不等式。  

化簡：  
\\(\left | x^2 -4 \right |\\)  
\\(=\left | (x-2) \right | \left | (x+2) \right | \\)  
\\(=\left | x-2 \right | \left |x+2 \right | < \varepsilon\\)  

要跟 \\(x-2\\) 取得關聯，最後的化簡是  
\\( \left |x-2 \right | = \displaystyle{\frac{\varepsilon}{\left |x+2 \right |}}\\)  
和第一個範例不同的是，式子的右邊仍是一個函式，所以我們需要針對 \\(x\\) 取一個範圍。

就定義來看，這裡的範圍怎麼取都無所謂，但就是不能脫離2這個點。我隨便訂個數字，例如前後範圍是1
好了。接著利用我們訂出來的範圍去估 \\(x+2\\) ，這是我們的目標。

Assume \\( \left |x-2 \right | < 1\\) （要記得這裡的範圍是自己訂的）  
Then \\( -1< x-2 < 1\\)  
\\( \Rightarrow 1< x < 3\\)  
\\( \Rightarrow 3< x+2 < 5\\)  
\\( \Rightarrow \left | x+2 \right | < 5\\)

因此我們可以再回到原本卡住的地方，利用這個訂出來的範圍繼續寫下去。  
\\(\left | x^2 -4 \right |\\)  
\\(=\left | x-2 \right | \left |x+2 \right |\\)  
\\(<5\left | x-2 \right |\\) （ \\(\because \left | x+2 \right | < 5\\) ）  
\\(< \varepsilon\\)  
\\( \Rightarrow \left | x-2 \right |< \displaystyle{\frac{\varepsilon}{5}}\\)  

這樣又可以回歸定義了，但是這裡要注意：  
我們剛剛有對 \\(x\\) 自己抓了一個範圍，但是無法確定自己抓的這個數值會不會比\\(\displaystyle{\frac{\varepsilon}{5}}\\)還小（畢竟要討論的是 \\(x\\) 的靠近，不能反而取一個不夠靠近的數值），所以取 \\(\delta\\) 時要特別取較小值以防這個情況：

Take \\(\delta = min(1,\displaystyle{\frac{\varepsilon}{5}})\\)  
Then \\(\forall x \; in \; 0 < \left | x-2 \right | < \delta,\;\left | x^2-4 \right | < \varepsilon\\)  
Therefore \\(\displaystyle{\lim_{x \to 2}x^2=4}\\)

## 後記
我記得大一時剛接觸證明的東西七零八落，也沒打算好好搞懂，現在回頭來看整個觀念終於串在一起了。未來若有遇到更多題型會再新增的。

---

## 參考
♦ [高淑蓉老師微積分（一）](https://www.youtube.com/watch?v=7qMu9OejpT8&list=PLS0SUwlYe8czw04JGine76IzoHc1MM8bO&index=8) 