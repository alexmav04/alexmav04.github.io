---
title: "偏微在影像處理的應用-Weickert's approach"
layout: single
author_profile: true
comments: true
categories:
  - math
tags:
  - image processing
  - partial differential equation
sidebar:
  nav: "sidepost"
---
# Joachim Weickert 
## Weickert's approach

$$\displaystyle{\frac{\partial u}{\partial t}}
= \mathrm{div}\left ( \begin{bmatrix}
a & b\\ 
b & c
\end{bmatrix} \nabla u \right )$$

同時可以注意到
$$\begin{bmatrix}
a & b\\ 
b & c
\end{bmatrix}$$ 此矩陣對稱，有求eigenvalue的好處。

Weickert希望去雜訊的同時，也可做修復。也就是說，在做擴散時也能去做選取的動作。
ex:指紋

## $$\frac{\partial u}{\partial t}$$ 推算
假設$$\begin{bmatrix}
a & b\\ 
b & c
\end{bmatrix}$$的eigenvalue為 $$\lambda_1,\lambda_2$$，對應的eigenvector為$$v_1,v_2$$

\\(\mathrm{div}\left ( \begin{bmatrix}
a & b\\ 
b & c
\end{bmatrix} \nabla u \right )\\)

\\(\displaystyle{=\frac{\partial}{\partial x}
\left ( 
a\frac{\partial u}{\partial x}+b\frac{\partial u}{\partial y}
\right )
+
\frac{\partial}{\partial y}
\left ( 
b\frac{\partial u}{\partial x}+c\frac{\partial u}{\partial y} 
\right )}\\)

\\(\displaystyle{\overset{claim}{=}\frac{\partial}{\partial v_1}
\left ( 
\lambda_1\frac{\partial u}{\partial v_1}
\right )
+
\frac{\partial}{\partial v_2}
\left ( 
\lambda_2\frac{\partial u}{\partial v_2}
\right )}\\)

其中擴散係數會跟eigenvalue有關係。

## 結構張量（structure tensor）
長得像
$$T=
\begin{pmatrix}
{u_{x_1}}^2 & u_{x_1}u_{x_2}\\ 
u_{x_1}u_{x_2} & {u_{x_2}}^2
\end{pmatrix}$$

這種樣子的我們叫它結構張量

\\(F(\theta) = (\mathrm{d}(\theta)\cdot\nabla u)^2\\), where \\(\mathrm{d}(\theta) = (\cos\theta \; \sin\theta)\\)

\\(=\mathrm{d}(\theta)^t\cdot\nabla u\cdot \nabla u^t\cdot\mathrm{d}(\theta)\\)

$$=(\cos\theta \; \sin\theta)
\begin{pmatrix}
{u_{x_1}}^2 & u_{x_1}u_{x_2}\\ 
u_{x_1}u_{x_2} & {u_{x_2}}^2
\end{pmatrix}
\begin{pmatrix}
\cos\theta \\ 
\sin\theta
\end{pmatrix}$$ .

在這裡  
eigenvalues: 
$$\lambda_1=|\nabla u|^2$$, $$\lambda_2 = 0$$

eigenvectors:
$$\displaystyle{v_1 = \frac{\nabla u}{|\nabla u|}}$$, $$\displaystyle{v_2 = \frac{\nabla u^\perp}{|\nabla u^\perp|}}$$

基本上相對於法向量來說，我們會希望沿著切線方向做擴散，才不會模糊到邊界。


## 用Gaussian Filter做預處理
概念有點像是先用熱方程先去做處理、去掉一些雜訊。在這裡我們用convolution的方式，通常做完convolution後，函數的光滑性會提高。

$$u_\rho=G_\rho *u$$，其中 $$G_\rho$$ 就是Gaussian filter

$$J_\rho=G_\rho *
\begin{pmatrix}
{(u_\rho)_{x_1}}^2 & (u_\rho)_{x_1}(u_\rho)_{x_2}\\
(u_\rho)_{x_1}(u_\rho)_{x_2} & {(u_\rho)_{x_2}}^2
\end{pmatrix}$$

假如說 $$\mu_1, \mu_2$$是 $$J_\rho$$ 的eigenvalues

這裡有幾種經典的情況可以討論：（這個要再看一下，不太確定啥意思）
1. $$\mu_1 \approx \mu_2 \approx 0$$，此種狀況圖像平坦。
2. $$\mu_1 \gg \mu_2 \approx 0$$，此種狀況表示在邊緣。
3. $$\mu_1 \geq \mu_2 > 0$$，平坦和邊緣部分各占了一部分的比例。

## 再回到最初的Weikert's
將最初的式子用 $$\mathrm{D}(J_\rho)$$ 取代

$$\displaystyle{\frac{\partial u}{\partial t} = \mathrm{div}(\mathrm{D}(J_\rho)\cdot\nabla u)}$$

其中 $$\mathrm{D}(J_\rho)$$ 的eigenvector還是跟 $$J_\rho$$ 一樣，而eigenvalue，也就是先前提到會相關的diffusion way已經不同了。

* Edge enhancing anisotropic diffusion  
  （我完全忘記為什麼筆記會蹦出這個）

  $$
  \lambda_1=
  \begin{cases}
  1 & \text{ if } \mu_1=\mu_2 \\
  1-\mathrm{exp}\left ( \displaystyle{\frac{-3.315}{ {\mu_1}^4} }\right ) & \text{ otherwise. }
  \end{cases}
  $$ .

  \\(\lambda_2=1\\)


* Coherence enhancing anisotropic diffusion  
  \\(\lambda_1=\alpha\\)  
  $$\lambda_2=
  \begin{cases}
  \alpha & \text{ if } \mu_1=\mu_2 \\ 
  \alpha-(1-\alpha)\mathrm{exp}\left (\displaystyle{\frac{-1}{(\mu_1-\mu_2)^4}} \right ) & \text{ otherwise. }
  \end{cases}$$
  $$\text{where }\alpha \in (0,1).$$


## 寫回矩陣
這堂課的目的原本就是要將這些式子用code去實作，進而看處理過的圖片是不是和預想的差不多。既然如此就必須要知道，一旦有了eigenvalue和eigenvector，要如何寫回矩陣。

$$\begin{bmatrix}
a & b\\
c & d
\end{bmatrix}=
\lambda_1 v_1 {v_1}^t + \lambda_2 v_2 {v_2}^t$$

---
暫時寫至這裡，未來再補充。

---
## 參考資料
* [Anisotropic Diffusion in Image Processing](https://www.mia.uni-saarland.de/weickert/Papers/book.pdf)
* [Structure tensor](https://en.wikipedia.org/wiki/Structure_tensor)
* [摺積](https://zh.wikipedia.org/wiki/%E5%8D%B7%E7%A7%AF)