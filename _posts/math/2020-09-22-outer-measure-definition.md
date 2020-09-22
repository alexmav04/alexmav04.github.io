---
title: "[real analysis]outer measure定義"
layout: single
author_profile: true
comments: true
categories:
  - math
tags:
  - definition
  - real analysis
  - outer measure
---
好久沒好好整理筆記了。  
最近終於讀到了傳說中的實分析，前期蠻多都要呼應到高等微積分（這時候Rudin當工具書很好用），原則上如果時間足夠的話，我會希望我自己也能在這邊多補充一點前置概念，畢竟我自己就是基礎打得不怎麼好的人，當然也希望能就同病相憐的人一把啦XD  
但礙於時間關係，我的方式大概跟之前一樣，先把學習筆記先弄上來再說。所以如果有不清楚的部份，很歡迎留言讓我知道，這將會成為我很棒的學習機會。

先來個熱騰騰的定義。

## Definition (outer measure)
\\(\text {Let } E \subseteq \mathbb{R} \text { be any subset.}\\ \\)  
\\(\text {Denote the outer measure of } E \text { by } m_*(E). \\ \\)

\\(m_*(E):=\inf \left \\{ \displaystyle{\sum^{\infty}_{j=1} \left \| Q_j \right \| : E \subset \bigcup _{j=1}^{\infty} Q_j } \right \\} \\)

\\(\text {where } Q_j \text {\'s are closed cubes in } \mathbb{R^n} \\)

### 白話解釋
找到可以蓋住E的所有cube（也就是\\(Q_j\\)），取cube體積加總後最小的那個。

當然，可以無限做這種取cube的動作，不一定每個cube都要一樣大小，以確保有好的inf可以取（總而言之就是無限逼近你想要的區域）。  
之後有空的話我會畫個簡單的圖，不過我相信會看到這篇的，想像這部份的圖應該不至於太困難。

### Remarks
* \\(0 \leq m_*(E) \leq \infty \\)
* 要注意我們在outer measure 的定義中取的範圍是「無限多組」cube，也就是說，如果把無限改成有限個，並不會是原先定義中的outer measure。然而這也是另有一個名稱，下面將要介紹到。

## Definition (Jordan content)
\\(\text{Let } J_*(E) \text{ be the outer Jordan content of } E \subseteq \mathbb{R}^n \\)

\\(J_*(E):=\inf \left \\{ \displaystyle{\sum^{J}_{j=1} \left \| Q_j \right \| : E \subset \bigcup _{j=1}^{J} Q_j } \right \\} \text{for some } J \in \mathbb{N} \\)


未完待續，我先去養個肝。

