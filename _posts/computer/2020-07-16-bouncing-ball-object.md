---
title: "bouncing ball 實作"
layout: single
author_profile: true
comments: true
categories:
  - computer
tags:
  - JavaScript
  - canvas
  - object
sidebar:
  nav: "sidepost"
---
一直以來很常聽到「物件」，但很少去真的實作過。為了補足這方面的概念，我用MDN裡[物件建構實作](https://developer.mozilla.org/zh-TW/docs/Learn/JavaScript/Objects/Object_building_practice)的例子去理解。我認為這是最初階、最易懂、也最有成就感的理解素材了。

建立物件前
我會先想一些必要元素、要設定的項目

元素：
1. 畫布的背景
2. 球
   1. 球的速度
   2. 球的樣子（外觀、位置）
3. 球的運動和碰撞牆壁反應
4. 因為想要小球各在不同的地方開始彈跳、也希望每個小球都有不一樣的速度和大小，所以需要設一個隨機取值的東西

元素都有了，可以開始基本設置了。

## html
網頁點開要有畫布、標題，以及引入js檔

```html
<!DOCTYPE html>
<html>
<head>
  <meta http-equiv="Content-Type" content="text/html; charset=utf-8" />
  <title>Bouncing Balls</title>
  <meta charset="utf-8" />
</head>
<body>
  <h1>bouncingggggg!!</h1>
  <canvas></canvas>
  <script src="mainjs.js"></script>
</body>
</html>
```

## javascript

接下來編輯javascript檔

### 設定random
我想要的功能是輸入最小值和最大值後，可以幫我任意挑出這兩個數之間的某個數字。

Math.random() : 回傳浮點數 x，其中 $$0 \leq x < 1$$。

Math.floor(x)：回傳小於等於x的最大整數。

```js
function random(min, max) {
  var num = Math.floor(Math.random() * (max - min)) + min;
  return num;
}
```

### 製造一顆球
要讓球動來動去，首先要有球啊！  
一顆新的球要有位置、大小（半徑）、速度、顏色

```js
function Ball() {
  this.x = random(0, width);
  this.y = random(0, height);
  this.velX = random(-7, 7);
  this.velY = random(-7, 7);
  this.color = 'rgb(' + random(0, 255) + ',' + random(0, 255) + ',' + random(0, 255) + ')';
  this.size = random(10, 20);
}
```

x, y是球的位置；velX和velＹ是球的速度；color和size應該就不用多講了。  
每個屬性都有加入random，讓每個球都不一樣。

### 讓球被畫出來
我們要利用canvas畫出這個球。  
arc(x, y, radius, startAngle, endAngle, anticlockwise)
```js
Ball.prototype.draw = function () {
  ctx.beginPath();
  ctx.fillStyle = this.color;
  ctx.arc(this.x, this.y, this.size, 0, 2 * Math.PI);
  ctx.fill();
}
```

### 球的即時運動狀態（碰到牆壁）
像動畫的影格概念。要讓東西動起來，事實上就是讓他的位置隨著時間的增加而變動。  
接著考慮例外狀況：碰到牆了怎麼辦？要怎麼判斷有沒有碰到牆？  
還記得前面設定了球的速度和位置嗎？這時候就派上用場了。  

以x方向為例子：

要判斷有沒有碰右邊的牆，利用球的半徑、目前位置，去跟畫布的寬度比較

![bouncing-ball-01](https://raw.githubusercontent.com/alexmav04/alexmav04.github.io/master/_posts/computer/img/bouncing-ball-01.jpg)

如果碰到牆，x方向的速度就設為目前速度的反方向（回彈的概念）
```js
  if ((this.size + this.x) >= width) {
    this.velX = -(this.velX);
  }
```

同樣道理，去判斷有沒有碰左邊的牆
```js
  if ((this.x - this.size) <= 0) {
    this.velX = -(this.velX);
  }
```
至於y方向的大同小異。

設定完後，我們剛剛僅是把速度更改，還沒有告訴它要怎麼利用。  
先前提到，動畫簡單講就是讓東西的位置隨著時間而變動。  
所以希望每次呼叫這個函數時（也就是碰到牆時），能夠依修正的速度變更位置。  
```js
  this.x += this.velX;
  this.y += this.velY;
```

因此這部分整體會長這樣：
```js
Ball.prototype.update = function () {
  if ((this.size + this.x) >= width) {
    this.velX = -(this.velX);
  }
  if ((this.x - this.size) <= 0) {
    this.velX = -(this.velX);
  }
  if ((this.size + this.y) >= height) {
    this.velY = -(this.velY);
  }
  if ((this.y - this.size) <= 0) {
    this.velY = -(this.velY);
  }
  this.x += this.velX;
  this.y += this.velY;
}
```

### 讓這些動作重複執行
這部分會用到以下方法↓

[requestAnimationFrame()](https://developer.mozilla.org/zh-TW/docs/Web/API/Window.requestAnimationFrame)：這個方法通知瀏覽器我們想要產生動畫，並且要求瀏覽器在下次重繪畫面前呼叫特定函數更新動畫。

首先我們弄出個空間放置這些球：

```js
let balls = [];
```

接著要先初始化小球，放進儲存空間，並限制小球的顆數（我設置25顆）：

```js
  while (balls.length < 25) {
    let ball = new Ball();
    balls.push(ball);
  }
```

再來就必須利用上面建好的函式讓這些小球開始動作了  
每一顆球都給它畫一次

```js
  for (i = 0; i < balls.length; i++) {
    balls[i].draw();
    balls[i].update();
  }
```

最後一部份寫起來像這樣：

```js
var balls = [];
function loop() {

  while (balls.length < 25) {
    var ball = new Ball();
    balls.push(ball);
  }
  for (i = 0; i < balls.length; i++) {
    balls[i].draw();
    balls[i].update();
  }
  requestAnimationFrame(loop);
}
```
但前面說過，動畫是位置隨著時間演進。但如果沒有消除前一個瞬間的東西，下個瞬間又畫上去，那就單純只是畫筆：

所以我們必須在繪製下一個畫格之前，覆蓋一個畫格。

利用 [fillRect(x, y, width, height)](https://developer.mozilla.org/zh-TW/docs/Web/API/Canvas_API/Tutorial/Drawing_shapes)

```js
ctx.fillRect(0, 0, width, height); 
```

不過這樣似乎有點單調，想加入一個小尾巴的效果。  
這裡是用再疊加個透明層上去的方式，隨著時間增加，透明層加越多越不透明，先前產生的小球就會漸漸消失。如果是一顆在移動的球，效果看起來就會像彗星的尾巴一樣。

```js
ctx.fillStyle = 'rgba(0, 0, 0, 0.25)'; 
```

這個部分像是這樣：

```js
var balls = []; 
function loop() {
  ctx.fillStyle = 'rgba(0, 0, 0, 0.25)';
  ctx.fillRect(0, 0, width, height);

  while (balls.length < 25) {
    var ball = new Ball();
    balls.push(ball);
  }
  for (i = 0; i < balls.length; i++) {
    balls[i].draw();
    balls[i].update();
  }
  requestAnimationFrame(loop);
}
loop(); //記得執行
```

一般來說用網頁多少都會寫個css，不過這裡大部分都用canvas畫完了，css沒多少，就不拉出來講述了。

***

## 後記：
MDN對於剛接觸程式的自學者算是蠻友善的，雖然尚有一部分沒有中文化，我想透過範例加上查閱應該是不難懂，尤其想學程式學會英文是有必要的。說到中文化，MDN很鼓勵大家去幫忙新增各國語言翻譯，就算只翻到一半也沒關係，有興趣的可以試試看。

這個彈跳球的範例還能延伸到乒乓球，未來會再寫寫看。另外應該也有不少人看到這個例子想到能不能連球體互相碰撞也反彈？這就牽涉到更複雜的設定了，MDN裡面也有提到這些函式庫：  
[PhysicsJS](http://wellcaffeinated.net/PhysicsJS/)、[matter.js](https://brm.io/matter-js/)、[Phaser]( http://phaser.io/)  
這種物件建構很常用在遊戲製作上，幾乎可以說是基礎。對遊戲有興趣的可以多翻以上幾個網頁。

---
## 參考
♦ [物件建構實作](https://developer.mozilla.org/zh-TW/docs/Learn/JavaScript/Objects/Object_building_practice)

