---
title: "SVD 簡介"
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
本篇我將會簡單介紹 SVD 這個東西。

## SVD 之定義
通常我們會被要求去分析一個非常大的資料，假設為 \\(\mathbf{X} \in \mathbb{C}^{m \times n}\\)

$$\mathbf{X} = \begin{bmatrix}
 \vdots& \vdots& & \vdots\\ 
 \mathbf{x}_1& \mathbf{x}_2& \cdots& \mathbf{x}_n\\ 
 \vdots& \vdots& & \vdots \\
\end{bmatrix}$$

其中 \\(\mathbf{x}_k \in \mathbb{C}^m\\)。

在這裡討論的 \\(\mathbf{X}\\) 有兩種樣子，一種是瘦長型（\\(m \gg n\\)）和矮胖型（\\(m \ll n\\)）。

進入正題，所謂的 SVD 是 Singular Vector Decomposition，中文為奇異值分解。簡單來說是把一個矩陣拆開成兩個么正矩陣（下文有時候會直接用英文 unitary matrix）和一個對角矩陣相乘。

\\(\mathbf{X} = \mathbf{U} \mathbf{\Sigma} \mathbf{V}^*\\)

其中 \\(\mathbf{U} \in \mathbb{C}^{m\times m}\\) 以及  \\(\mathbf{V} \in \mathbb{C}^{n\times n}\\) 是由單範正交（orthonormal）column 所組成的 unitary matrix，而 \\(\mathbf{X} \in \mathbb{R}^{m\times n}\\) 是非負的對角矩陣。

---

**Notes**：
* 米字號 * 的意思
\\(\mathbf{A}^*\\) 是 \\(\mathbf{A}\\) 的共軛（複數）轉置矩陣（complex conjugate transpose）。
* unitary matrix:
是指一個方陣 \\(\mathbf{A}\\) 有下列性質：\\(\mathbf{A}^* \mathbf{A} = \mathbf{A} \mathbf{A}^* =\mathbf{I}\\)。

---

當 \\(m \ge n\\) 時（相對來是說瘦長型的）那麼應該可以很容易推知 \\(\mathbf{\Sigma}\\) 最多只會有 \\(n\\) 個元素是非零的，原因是這個矩陣是對角矩陣。因此我們還可以把原式子改寫：

$$\begin{align*}
\mathbf{X} &= \mathbf{U} \mathbf{\Sigma} \mathbf{V}^*\\ 
 &= \hat{\mathbf{U}} \hat{\mathbf{\Sigma}} \mathbf{V}^*\\
\end{align*}$$

噢這當然看不出個所以然，符號請對照下圖XD

![SVD-intro-01](https://i.imgur.com/MKeLjAn.jpg)

因為 \\(\mathbf{\Sigma}\\) 的關係，\\(\mathbf{U}\\) 也能夠化簡。這種精簡版的拆解稱為 economy SVD。

## SVD 在電腦中的計算

老實說 SVD 放到電腦裡面算，指令方面出乎我意料地簡單又直觀。

* MATLAB

這裡的例子是將一個圖形 A 轉變為一個矩陣 X，並計算 X 的 SVD，直接寫`svd(X)`即可。

```c
X = double(rgb2gray(A));
[U,S,V] = svd(X);
[U,S,V] = svd(X, 'econ');
```
第3行的 `econ` 指的就是 economy。

這裡以隨機生成的 7x3 矩陣輸出結果給大家感受一下。

```c
>> X = randn(7,3)

X =

    0.8884   -0.7549   -0.8649
   -1.1471    1.3703   -0.0301
   -1.0689   -1.7115   -0.1649
   -0.8095   -0.1022    0.6277
   -2.9443   -0.2414    1.0933
    1.4384    0.3192    1.1093
    0.3252    0.3129   -0.8637
```
```c
>> [U,S,V] = svd(X)

U =

   -0.2577    0.4091    0.1601    0.2851    0.7736   -0.0077   -0.2465
    0.2525   -0.5143    0.4441    0.1632    0.0763    0.5178   -0.4171
    0.2833    0.6909   -0.0904   -0.0015   -0.2759    0.5909   -0.0941
    0.2397   -0.0454   -0.2169    0.9139   -0.1508   -0.1438    0.1215
    0.7937   -0.0428   -0.1674   -0.2180    0.4766   -0.0964    0.2371
   -0.2873   -0.2948   -0.7036   -0.0008    0.2487    0.5096    0.1181
   -0.1402    0.0023    0.4459    0.0963    0.0884    0.3049    0.8194


S =

    3.9485         0         0
         0    2.4308         0
         0         0    1.7984
         0         0         0
         0         0         0
         0         0         0
         0         0         0


V =

   -0.9652   -0.0187   -0.2608
   -0.0750   -0.9357    0.3448
    0.2505   -0.3524   -0.9017

```
```c
>> [Uhat,Shat,V] = svd(X,'econ')

Uhat =

   -0.2577    0.4091    0.1601
    0.2525   -0.5143    0.4441
    0.2833    0.6909   -0.0904
    0.2397   -0.0454   -0.2169
    0.7937   -0.0428   -0.1674
   -0.2873   -0.2948   -0.7036
   -0.1402    0.0023    0.4459


Shat =

    3.9485         0         0
         0    2.4308         0
         0         0    1.7984


V =

   -0.9652   -0.0187   -0.2608
   -0.0750   -0.9357    0.3448
    0.2505   -0.3524   -0.9017
```

---

## 參考資料
* [MATLAB程式設計：入門篇](http://mirlab.org/jang/books/matlabProgramming4beginner/)
* [DATA DRIVEN SCIENCE & ENGINEERING-Machine Learning, Dynamical Systems and Control](http://databookuw.com/)
* [Khan Academy - Alternate coordinate systems(bases)](https://www.khanacademy.org/math/linear-algebra/alternate-bases/othogonal-complements/v/linear-algebra-orthogonal-complements)