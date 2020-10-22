---
title: "[javascript]正規表示式中的問號?作用"
layout: single
author_profile: true
comments: true
categories:
  - computer
tags:
  - javascript
  - regex
sidebar:
  nav: "sidepost"
---
在正規表示式（或稱正則表達式）中的問號 `?` 作用是什麼？看了菜鳥工程師 肉豬的文章寫得很清楚，這邊用自己的話再做一次紀錄。

---

### 作用
表示問號前面的字可以出現1次或0次（可有可無）。所以問號前面的那個字有出現，匹配；問號前面那個字沒出現，也是匹配。

### 實際用法
上面講得有些籠統，實際看用法應該會比較清楚。

```js
Pattern p = Pattern.compile("https?");
System.out.println(p.matcher("https").matches()); //true，結尾有s，其他字都符合，匹配！
System.out.println(p.matcher("http").matches());   // true，結尾沒有s，其他字都符合，匹配！
System.out.println(p.matcher("httpss").matches());  // false
```
應該蠻容易看出來，我要測的是http和https（s可有可無），兩個我都接受，但httpss不是我要的東西。

至於若想判斷多一點字就用括號
```js
Pattern p = Pattern.compile("student(one)?");
System.out.println(p.matcher("studentone").matches()); // true
System.out.println(p.matcher("student").matches());  // true
System.out.println(p.matcher("studenttwo").matches());  // false
```
one只可以「有」或是「沒有」的狀態，包含其他字就無法匹配。


## 參考
♦ [Regex 問號 ? 的用法]( https://matthung0807.blogspot.com/2017/09/regex_18.html)