---
title: "[SVD]Matrix Approximation"
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
本篇我將會簡單介紹 SVD 和 low-rank approximation 的關係以及圖像上的應用。  
關聯篇章：[SVD 簡介](https://alexmav04.github.io/math/SVD-introduction/)

## 主要概念

這部分需要先釐清一些概念。

### 什麼是 low-rank？
中文應該是翻作「低秩」吧，若有錯歡迎指正。當有一個 mxn 矩陣的 rank 遠低於 m 和 n，我們會將這個矩陣稱作 low-rank。

### 和 SVD 有何關係？
手邊有個 \\(\mathbf{X}\\)，想求一個 rank 為 r 的矩陣 \\(\tilde{\mathbf{X}}\\)，能使 \\(\left \| \mathbf{X} - \tilde{\mathbf{X}} \right \|\\) 達到最小值。這裡的 \\(\tilde{\mathbf{X}}\\)，是截取 \\(\mathbf{X}\\) 前 r 行前 r 列的方陣。

那這個解會是什麼呢？答案便是 \\(\tilde{\mathbf{X}}\\) 的 SVD。用數學符號表示：

$$\displaystyle \mathop{\arg\min}_{\tilde{\mathbf{X}}, \text{ s.t. } \text{rank}(\tilde{\mathbf{X}}) = r }\| \mathbf{X} - \tilde{\mathbf{X}}\|_F = \tilde{\mathbf{U}}\tilde{\mathbf{\Sigma}}\tilde{\mathbf{V}}^*$$

其中 $$\left \| \cdot \right \|_F$$ 代表的是 Forbenius norm，而 \\(\tilde{\mathbf{U}}\\) 和 \\(\tilde{\mathbf{V}}\\) 是截取 \\(\mathbf{U}\\) 和 \\(\mathbf{V}\\) 的前 r 行。以上敘述是由理論而來的（Eckart-Young）。

因為 \\(\tilde{\mathbf{\Sigma}}\\) 是對角矩陣的關係，我們還可以把 \\(\tilde{\mathbf{X}}\\) 表示成以下式子：

$$\displaystyle \tilde{\mathbf{X}} = \sum_{k=1}^{r} \sigma_k\mathbf{u}_k\mathbf{v}_k$$


## 應用於圖像壓縮
事實上 SVD 在機器學習領域以及數值計算上都有相當重要的應用。這次我想談到的是在圖像處理方面，如何利用 SVD 將圖像壓縮。

### 原理
為什麼要利用 SVD 在圖像上？現今相片的畫素已經相當高了，一張照片所佔的容量和資訊其實相當之大。然而其實有很多「資訊」都是冗員。以灰階圖像為例子和大家說明：

假設 0 是黑的，1 是白的，其餘灰階數值分布在 0 到 1 之間。我們可以把下面這張圖用矩陣表示為
$$\mathbf{U} = \begin{bmatrix}
0 & 0.1 & 0.2 & 0.3 & 0.4 & 0.5 & 0.6 & 0.7 & 0.8 & 0.9
\end{bmatrix}^\intercal$$

![SVD-02](https://i.imgur.com/EDGdxQM.jpg)

將這張灰階圖複製 10 次（變成 10x10 矩陣）

![SVD-02](https://i.imgur.com/7rPspyV.jpg)

上面這張圖可以用矩陣表示成：

$$\mathbf{U}\mathbf{V}^* =\begin{pmatrix}
 0& 0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 & 0\\ 
 0.1& 0.1 & 0.1 & 0.1 & 0.1 & 0.1 & 0.1 & 0.1 & 0.1 & 0.1\\ 
 0.2& 0.2 & 0.2 & 0.2 & 0.2 & 0.2 & 0.2 & 0.2 & 0.2 & 0.2\\ 
 \vdots & \vdots & \vdots & \vdots & \vdots & \vdots & \vdots & \vdots & \vdots & \vdots\\ 
 \vdots & \vdots & \vdots & \vdots & \vdots & \vdots & \vdots & \vdots & \vdots & \vdots\\ 
 0.9& 0.9 & 0.9 & 0.9 & 0.9 & 0.9 & 0.9 & 0.9 & 0.9 & 0.9
\end{pmatrix}$$

其中 \\(\mathbf{V}\\) 是全部元素為 1 的 10x1 矩陣。

可以感受到第二張圖所表示的矩陣冗員很多，真正的資訊只有 \\(\mathbf{U}\\) 本身以及 \\(\mathbf{V}^*\\) 的資訊（重複），也就是說只有兩個向量需要儲存而已。

---

這是一個很簡單的例子讓大家感受，而實際上的圖片比這邊舉例的圖要複雜得多了，儘管如此我們猜想應該仍會有顏色非常相近的地方能夠去省略吧？雖然嚴格來講是 linear independent，但可以被看成是「近似」線性相關的，因為這種微小差異（顏色相近）對於圖片的整體特徵和架構來看並不會影響很大，而奇異值也會相對小。因為如此，我們可以捨去奇異值較小的那幾項，而這個矩陣就會被我們稱為 low-rank approximation。

知道背後原因後我們又該如何去最佳化這個資訊量呢？這時候 SVD 就是一個很好的方法。

### 實際操作

還記得在 [SVD 簡介](https://hackmd.io/@alexmav04/BkM8iU4Jd) 曾經特別提及能夠捨去一些矩陣中的冗員，並在此篇中談到我們可以略去奇異值較小的那幾項，我們便是利用這些特質將 SVD 在圖片壓縮上做一個很不錯的應用。

* MATLAB

首先我們先將讀取到的圖片轉成灰階圖，並放置到 4X4 格中的第一格，表示此為原始圖像。
```matlab
A = imread('./dog.jpg');
X = double(rgb2gray(A)); 

subplot(2,2,1);
imagesc(X), colormap gray, axis image;
title('Original');
```
定義 nx, ny 分別為 X 的 row 數以及 column 數，接著直接算 X 的 economy SVD。
```matlab
nx = size(X,1); ny = size(X,2);
[U,S,V] = svd(X, 'econ');
```
再來我們就要比對各個截斷效果以及壓縮率了，這邊的例子挑 r=5、r=20、r=100。

```matlab
plotind = 2;
for r=[5 20 100];
    Xapprox = U(:,1:r)*S(1:r,1:r)*V(:,1:r)';
    subplot(2,2,plotind), plotind = plotind + 1;
    imagesc(Xapprox), axis image;
    title(['r=',num2str(r,'%d'),', ',num2str(100*r*(nx+ny)/(nx*ny),'%2.2f'),'% storage']);
end
```
以下結果供大家參考：

![SVD-04](https://i.imgur.com/yAJ470f.jpg)


## 參考資料
* [MATLAB程式設計：入門篇](http://mirlab.org/jang/books/matlabProgramming4beginner/)
* [DATA DRIVEN SCIENCE & ENGINEERING-Machine Learning, Dynamical Systems and Control](http://databookuw.com/)