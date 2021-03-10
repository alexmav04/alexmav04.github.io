---
title: "[SVD] 直觀看待SVD及PCA之關係"
layout: single
author_profile: true
comments: true
categories:
  - math
tags:
  - SVD
sidebar:
  nav: "sidepost"
---
這篇要很粗淺地介紹什麼是 PCA、以及用很直觀的觀點去解釋 PCA，最後再用很簡單的方式討論 PCA 和 SVD 是什麼關係。

## 今日主角：PCA之介紹

### 前置觀念

PCA，全名 Principal Component Analysis ，中文名字叫做主成份分析。

在各種資料處理的方法中，降低維度是很重要的一環。

那麼隨著資料的成長，資料的維度勢必也越來越高，我們要處理的東西也會越來越複雜。通常資料只要大於五維，以視覺化甚至是人類的想像來說已經相當吃力了。更何況我們可能會碰觸到50維、100維度以上，這時候我們很直觀會思考的步驟便是如何簡單而有效地降低維度。

收集的資料量越來越大，這些資料都是我們所需要的嗎？思考這個問題的同時我們便是在考慮捨去不必要的元素。

主成份分析，顧名思義，一件東西需要分析出最主要的成份。通常我們會拿到的「東西」可能會是龐大的資料、一張圖等等。那麼什麼叫做最主要的成份呢？可以用以下例子如此想像：

假設我們現在有近視，當遠處有位朋友走來，我們即使沒帶眼鏡仍能夠認出這位朋友對吧？好吧我有輕微臉盲所以也不一定XD 撇除掉這種特例，可以得知即使這位朋友臉的解析度沒那麼好（在此絕非委婉批評醜度，而是真的指影像較為模糊），我仍能辨識此位朋友。即便是朋友改變髮型或是膚色。

由以上例子可以大概得知，以辨識人臉來說，眼睛、鼻子、嘴巴甚至到臉的輪廓的形狀或相對位置等等的是相當重要的資訊，然而髮型、膚色相對來說並沒有這麼重要，應該可以捨去，也不會影響到我們辨識人的能力。

---
題外話，記得泰絲．格里的小說《骸骨花園 The Bone Garden》中最後一段 Rose 看著一幅人像畫說道：
> 你總是能一眼認出你所愛的人

嗯﹍不過今天「愛」這個元素不在討論範圍內喔XD

### 關注的重點

本篇主要是用很直觀的面向去帶入 SVD 和 PCA 的關係，因此不會探討到太深入的概念，僅就我們需要的部份去解釋。最主要需要討論的部份，同時也是 PCA 的精髓，便是上頭提到那兩項：如何降低維度以及如何決定和去除不必要的元素。

* 降維

先來談談降維這檔事。為了方便理解推導，我們會先討論最最最簡單的降維：二維降到一維。在我們所理解的座標系統中，就是由平面降到一條線對吧，然而是怎麼執行的呢？

![SVD-and-PCA-1](https://i.imgur.com/4n7iv6I.jpg)

首先，假設我們有一組資料，每一個樣本點都有兩個特徵，也就是二維的資料。直觀上來\frac{\partial }{\partial x}講我們會很直覺地將它們畫在我們所熟悉的座標平面上。

![SVD-and-PCA-2](https://i.imgur.com/wD0H8wo.jpg)

看得出來資料分佈在二維平面上了。

接下來希望它能壓在一維的線上，直覺上來說會畫在資料分佈的趨勢上，但在討論最佳化這條線之前，其實怎麼畫都行。為了避免太複雜的計算以及貫徹向量的概念，我們會將這條線從 0 出發開始（其實不從 0 出發也可以，因為我們討論的是方向性，平移是沒差的）。

既然是要壓縮在直線上，所要討論的就是一個點在線上的投影。

* 去除不必要的元素

這部份就需要借助 SVD 的威力了，稍後會說明。

## PCA 直觀的推導

既然知道原理了，簡單來說就是降維和去除多餘的元素，剩下一個大重點（其實細節還很多，不過先忽略哈哈），就是我們要如何取得最好的向量？

### 我們要怎麼選最好的向量

首先，直覺上來講，一定會希望向量和資料的差距越小越好，也就是說希望上圖紅色線段加總起來能夠越小越好。

> 要注意的是，雖然聽起來和線性回歸很像，但是概念卻差很多。線性回歸是要做預測，希望 y 值能夠越接近預測越好，所以在意的是相同的 x 上，資料的 y 值和線上所預測的 y 的距離。而 PCA 不做預測，而是就現有的資料做最好的向量，所需要的是上面我們所講的投影距離。
> 
![SVD-and-PCA-3](https://i.imgur.com/gxSBu6g.jpg)

再來，我們會希望變異越大越好。這個乍聽之下很違反直覺，不過冷靜思考一下應該很快能夠理解。所謂變異越大，在圖上的意思是分佈越大，我們希望資料集之間的區別和分佈能夠保留。想像一下，若是轉變一個座標系統，而資料集卻通通擠在一起，那麼原本資料的特性未被保留，降維是否就沒什麼意義了？

因此接下來我們要專注在兩件事：
1. 錯誤越小越好
2. 變異越大越好

以上兩點在資料處理上會很常看到，換句話說就是資料能夠保留的原始資訊越多越好。

### 錯誤越小越好

這部分之後有空再說。

### 變異越大越好

先來回顧簡單的投影概念：

![SVD-and-PCA-4](https://i.imgur.com/LXilbvI.jpg)

投影後內積的值會是：

\\(\langle x_i, v\rangle = v^\intercal x_i\\)

我們知道單變數的變異數的求法：

\\(\displaystyle\sigma ^2 = \frac{1}{n} \sum_{i=1}^{n}(v^\intercal x_i - \mu)^2\\)

其中 \\(\mu\\) 是平均值。

這裡我們會考慮 \\(\mu = 0\\)，如同先前所提，因為我們關注的是 \\(v\\) 的方向性，平移一個向量對我們來說沒什麼差，再說，\\(\mu = 0\\) 的狀況對計算上簡化很多，因此會先做這個前提假設，之後再回去看 PCA 的條件。 

再來將它轉換成多變數的型態：

$$\begin{align*}
\displaystyle\Sigma &= \frac{1}{n} \sum_{i=1}^{n}(v^\intercal x_i)(v^\intercal x_i)^\intercal\\ 
 &= \frac{1}{n} \sum_{i=1}^{n}(v^\intercal x_i x_i^\intercal v)\\
 &= v^\intercal \left( \frac{1}{n} \sum_{i=1}^{n} x_i x_i^\intercal \right) v
\end{align*}$$

有沒有發現中間括號的部份很眼熟！這也可以看作是另一組變異數，之後會使用到這個概念，這裡先將他設為 \\(C\\)。上式可以看作是：\\(v^\intercal C v\\)。

還記得我們希望變異越大越好嗎？在數學上習慣將這句話寫作：

\\(v = \mathop{\rm argmax}\limits_{\|v\| = 1}v^\intercal C v\\)

意思是求出能夠使 \\(v^\intercal C v\\) 達到最大值的 \\(v\\)，也就是我們朝思暮想的最好的向量。

有條件限制和幾個變數的最佳化問題，可以用 Lagrange multiplier 方法：

$$\begin{align*}
f(v,\lambda) &= v^\intercal C v - \lambda(\|v\|-1) \\&= v^\intercal C v - \lambda(v^\intercal v - 1)
\end{align*}$$

對 \\(v\\) 做偏微分後，可得到：

\\(2vC - 2\lambda v = 0\\)

移項處理一下，有一點線性代數概念的人應該很熟悉這個算式，沒錯，最終原來是我們要求 eigenvector 以及 eigenvalue 啦：

\\(Cv = \lambda v\\)

### 總結 PCA 之步驟

有了以上很直覺式的推導，我們可以大致上總結出 PCA 的步驟，其實也大概是實際我們會做的事情：

\1. **數據標準化**

記得我們中間有 \\(\mu = 0\\) 的假設前提嗎？不要這個前提忘了，因此我們需要先將資料標準化。

\2. **建立共變異矩陣**

這個步驟應該毫無懸念。

\3. **利用 SVD 求得 eigenvector 和 eigenvalue**

關於此步驟後面會提到。

\4. **將 eigenvalue 由大到小排列，選取前 k 個 eigenvector 和 eigenvalue**

其實就是 SVD 之後的 \\(\mathbf{\Sigma}\\) 中的奇異值，我們選取前 k 個意味著我們選了前面 k 大的奇異值。

\5. **將原本的數據投影到 eigenvector，得到新的特徵**

找到好的向量後，這步驟便是我們最終目的。

## SVD 以及 PCA 之掛勾

講解完 PCA 簡單的原理，那麼我們要思考的是，為什麼講到 PCA 時總是會提起 SVD 呢？

### SVD 簡單介紹

可參考我先前寫的簡單介紹：[SVD 簡介](/al3zF693RjaqezEsozdAGA)

借用一點文字過來 recall 一下 XD

所謂的 SVD 是 Singular Vector Decomposition，中文為奇異值分解。簡單來說是把一個矩陣拆開成兩個么正矩陣和一個對角矩陣相乘。

\\(\mathbf{X} = \mathbf{U} \mathbf{\Sigma} \mathbf{V}^*\\)

其中 \\(\mathbf{U} \in \mathbb{C}^{m\times m}\\) 以及  \\(\mathbf{V} \in \mathbb{C}^{n\times n}\\) 是由單範正交（orthonormal）column 所組成的 unitary matrix，而 \\(\mathbf{\Sigma} \in \mathbb{R}^{m\times n}\\) 是非負的對角矩陣。



### 所以如何牽扯

為了觀察它們的關係，在這裡動些小手腳，推導後可以得知他們兩個其實是相輔相成的東西：

\\(\mathbf{A} = \mathbf{U}\mathbf{\Sigma}\mathbf{V}^\intercal\\) 代入

$$\begin{align*}
\mathbf{A}^\intercal \mathbf{A} &= \left( \mathbf{U}\mathbf{\Sigma}\mathbf{V}^\intercal \right)^\intercal\left(\mathbf{U}\mathbf{\Sigma}\mathbf{V}^\intercal\right)\\
&=\mathbf{V}\mathbf{\Sigma}^\intercal\mathbf{U}^\intercal\mathbf{U}\mathbf{\Sigma}\mathbf{V}^\intercal\\
&=\mathbf{V}\mathbf{\Sigma}^2\mathbf{V}^\intercal
\end{align*}$$

蠻容易看出來，\\(\mathbf{V}\mathbf{\Sigma}^2\mathbf{V}^\intercal\\) 就是 \\(\mathbf{A}^\intercal \mathbf{A}\\) 的分解，另外觀察一下對角矩陣剛好差一個平方，也就是說 \\(\mathbf{A}^\intercal \mathbf{A}\\) 的奇異值 \\(\sigma_i\\) 剛好也會是 \\(\mathbf{A}\\) 的特徵值的平方根 \\(\sqrt{\lambda_i}\\) 。

到這裡相信大家都看出來，最後推導出來的東西實在是跟剛剛還在討論 PCA 時的某個東西相當相似。這個感覺沒有錯，在討論 PCA 時，我們關注的是 \\(\displaystyle\frac{1}{n} \sum_{i=1}^{n} x_i x_i^\intercal\\)，噢我們這裡先將他寫成矩陣的樣子好了，\\(\displaystyle\frac{1}{n} \mathbf{X}  \mathbf{X}^\intercal\\) 的分解，而在 SVD 我們關注的是 \\(\mathbf{A}^\intercal \mathbf{A}\\) 的分解。

現在我們假設 \\(\displaystyle\mathbf{A} = \frac{\mathbf{X}^\intercal}{\sqrt{n}}\\) ，並利用它代回去上面式子：

$$\begin{align*}
\displaystyle\mathbf{A}^\intercal \mathbf{A} &= \left(\frac{\mathbf{X}^\intercal}{\sqrt{n}}\right)^\intercal\left(\frac{\mathbf{X}^\intercal}{\sqrt{n}}\right)\\
&=\frac{1}{n} \mathbf{X}  \mathbf{X}^\intercal
\end{align*}$$

喔幹，超讚。完完全全地可以把它們倆牽扯在一起了。

### 牽扯到 SVD 之原因

做了這麼多，就是為了要讓 SVD 的方法能夠應用在 PCA 的概念中，但究竟是什麼好處？這是我最後想分享的。

* 維度差別

前面提到過，在 PCA 中，我們最後注意的是 \\(\mathbf{A}  \mathbf{A}^\intercal\\) 的分解，而在 SVD 我們關注的是 \\(\mathbf{A}^\intercal \mathbf{A}\\) 的分解。一般我們進行的是瘦長型矩陣，這時候 \\(\mathbf{A}  \mathbf{A}^\intercal\\) 的計算量會非常之大，效率便不會好到哪裡去。

* 資料精省

我不太清楚要怎麼用中文精準地表達這個動作XD 在 SVD 中有個很好的特性是它能夠精簡我們要儲存的矩陣。

![SVD-and-PCA-5](https://i.imgur.com/U1fi9iE.jpg)

對照著上圖，跟著以下文字一起思考：

首先，我們知道 \\(\mathbf{\Sigma}\\) 是對角矩陣，因此原則上  \\(\mathbf{\Sigma}\\) 最少會有下面 \\(m-r\\) 個 row 都是 0 元素。因為這個性質，我們知道 \\(\mathbf{U}\\) 後面至少 \\(m-r\\) 個 column 是不需要理會的（反正矩陣相乘了也是0）。

再來，如同前面所說，只需要取前 r 個重要的資料，因此 \\(\mathbf{\Sigma}\\) 又縮小了。既然 \\(\mathbf{\Sigma}\\) 縮小，勢必 \\(\mathbf{U}\\) 有作用的範圍也會變小，而 \\(\mathbf{V}\\) 也是。

最後，我們實際上所需儲存的矩陣其實比起當初原始矩陣還要小多了，自然在機器中處理上較有效率。

## 參考資料
* [深入理解PCA与SVD的关系](https://zhuanlan.zhihu.com/p/58064462)
* [世上最生動的 PCA：直觀理解並應用主成分分析](https://leemeng.tw/essence-of-principal-component-analysis.html)
* [拉格朗日乘數](https://zh.wikipedia.org/wiki/%E6%8B%89%E6%A0%BC%E6%9C%97%E6%97%A5%E4%B9%98%E6%95%B0)