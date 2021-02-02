---
title: "[Python]爬取英文單字存成csv檔"
layout: single
author_profile: true
comments: true
categories:
  - computer
tags:
  - Jupyter
  - Python
  - web crawler
sidebar:
  nav: "sidepost"
---
這次我想爬取 [majortests](https://www.majortests.com/word-lists/) 中提供的英文單字。這個網站的組成蠻簡單的（雖然廣告很多），標籤也很一致且整齊，很適合做為爬取單字的爬蟲練習。

## 前置作業－觀察

首先我們要先觀察這個網頁的樣態。可以發現這個網頁中的單字表還有分1~10個單元，這次的目的是希望把這幾個單字表的單字都通通抓取下來，因此我們會需要這10個單字表的網址。

![web-crawler-01](https://i.imgur.com/pUE2a67.jpg)

實際查看後發現，這幾個單字表的網址變化蠻規律且簡單的。

```
https://www.majortests.com/word-lists/word-list-01.html
https://www.majortests.com/word-lists/word-list-02.html
https://www.majortests.com/word-lists/word-list-03.html
https://www.majortests.com/word-lists/word-list-04.html
https://www.majortests.com/word-lists/word-list-05.html
https://www.majortests.com/word-lists/word-list-06.html
https://www.majortests.com/word-lists/word-list-07.html
https://www.majortests.com/word-lists/word-list-08.html
https://www.majortests.com/word-lists/word-list-09.html
https://www.majortests.com/word-lists/word-list-10.html
```

整齊、好改，很讚。

接著需要觀察單字表在 html 中的標籤型態，我們才能依據標籤爬取正確資訊。隨意點一個單字表並且按 F12 開啟開發人員工具。

![web-crawler-02](https://i.imgur.com/b8OcY3b.jpg)

對照實際的單字表：

![web-crawler-03](https://i.imgur.com/KPIFiFr.jpg)

可以觀察到單字表是存在 `table` 標籤中，其中的 class 為 `wordlist`，再來單字表有單字和解釋，分別在 `tr` 標籤中用 `th` 以及 `td` 隔開，蠻容易辨識的，待會寫起來也不會太麻煩。

## 前置作業－功能

接著想想看要有什麼 function 去執行這些事情。

* **生成網址**

首先一定是生成網址，網址的型態為：

```python
URL = "https://www.majortests.com/word-lists/word-list-0{0}.html"
```

因為標號是 01, 02, ....,09, 10，想將過程再弄得更簡單易點，這次我們只抓取第 1~9 個單字表，就暫時不用思考進位後兩個數字怎麼變的問題。

* **改變 header**

有了網址，也有了 html 標籤分析，差不多可以寫最基本的爬蟲了。
但網站有可能會發現是 python 在拋出請求，為了不讓網站發現，需要一個 function 去將 header 更改成瀏覽器的樣子。

* **獲取網頁 tag 結構**

同時也必須要能夠解析抓取到的 html，並將這個文本丟給自己寫的 function，讓它可以找自己訂下來的 html tag 去抓取。

* **抓取單字**

開始利用 BeautifulSoup 所獲得的 tag 結構，去找尋自己所需要的東西。

* **爬蟲**

簡單爬蟲。利用以上的 function，將所要爬的網址改變 header 後，取得該網站結構，並且利用這個結構去找自己要的 tag，並儲存單字在自己設的陣列裡面。

* **最後存成 csv 格式**

最後要有個 function 將收集到的資料寫成 csv 檔。

## 實際作業

規劃了大概的樣子，差不多可以開始寫了。
首先要 import 的東西有：

```python
from bs4 import BeautifulSoup
import time
import csv
import requests
```

### 生成網址

**`generate_urls`**

首先網址為：

```python
URL = "https://www.majortests.com/word-lists/word-list-0{0}.html"
```

設一個陣列 `urls`，用迴圈生成編號從1~9的網址，並將生成的網址放進裡面。

```python
def generate_urls(url, start, end):
    urls = []
    for i in range(start, end + 1):
        urls.append(url.format(i))
    return urls
```

### 改變 header

複製任何一種查得到的 header 都可以，只要能順利讓網站接受即可

```python
def change_get(url):
    headers = {'user-agent': 'Mozilla/5.0 (Windows NT 10.0; Win64; x64)'
           'AppleWebKit/537.36 (KHTML, like Gecko)'
           'Chrome/63.0.3239.132 Safari/537.36'}
    return requests.get(url, headers = headers)
```

### 獲取網頁 tag 結構

獲得 html tag 結構

```python
def get_html(str):
    return BeautifulSoup(str, "lxml")
```

### 抓取單字

```python
def get_words(soup, file):
    words = []
    count = 0
    
    for table in soup.find_all(class_ = "wordlist"):
        count += 1
        for word_entry in table.find_all("tr"):
            newWord = []
            newWord.append(file)
            newWord.append("Group " + str(count))
            newWord.append(word_entry.th.text)
            newWord.append(word_entry.td.text)
            words.append(newWord)
    return words
```

### 爬蟲

```python
def web_scraping(urls):
    eng_words = []
    
    for url in urls:
        file = url.split("/")[-1].split(".")[-2].replace('-', ' ')
        print("scrape the file : " + file)
        r = change_get(url)
        if r.status_code == requests.codes.ok:
            soup = get_html(r.text)
            words = get_words(soup, file)
            eng_words = eng_words + words
            print("wait for 5 seconds...")
            time.sleep(5)
        else:
            print("HTTP requests error")
    return eng_words
```

### 最後存成 csv 格式

```python
def to_csv(words, file):
    with open(file, "w+", newline = "", encoding = "utf-8") as fp:
        writer = csv.writer(fp)
        for word in words:
            writer.writerow(word)
```

## 最終成果

主程式：

```python
if __name__ == "__main__":
    urls = generate_urls(URL, 1, 9)
    eng_words = web_scraping(urls)
    for item in eng_words:
        print(item)
    to_csv(eng_words, "words.csv")
```

還行，看起來有模有樣，不用自己建單字表建得那麼辛苦了XD
但發現有很多不會的單字還真是有點汗顏……

![web-crawler-03](https://i.imgur.com/OoXuRCw.jpg)

## 參考資料
* [\[Python爬蟲教學\]7個降低Python網頁爬蟲被偵測封鎖的實用方法](https://www.learncodewithmike.com/2020/09/7-tips-to-avoid-getting-blocked-while-scraping.html)