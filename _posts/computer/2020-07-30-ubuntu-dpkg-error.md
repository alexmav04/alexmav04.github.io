---
title: "[Ubuntu]錯誤訊息:ub-process /usr/bin/dpkg returned an error code (1)"
layout: single
author_profile: true
comments: true
categories:
  - computer
tags:
  - Ubuntu
  - system
  - error
---
安裝套件時一直出現這個錯誤訊息。  
這篇解決方法是參考這裡:[解決錯誤Sub-process /usr/bin/dpkg returned an error code (1)](https://sites.google.com/a/cnsrl.cycu.edu.tw/da-shu-bi-ji/solve-bug/jiejuecuowusub-processusrbindpkgreturnedanerrorcode1)

完全照做，知道步驟在幹嘛但是不太了解原理，暫時紀錄以後再回過頭來補充。

先把info資料夾備份起來(取啥名字都沒差)，和參考文章比較不一樣的是這裡我是直接`cd /var/lib/dpkg`進去裡面操作，就不用一直重新打路徑。
```
sudo mv info info.bak
```

再新建一個資料夾叫做info
```
sudo mkdir info
```

更新並安裝套件
``` 
sudo apt update
apt -f install <your package> #這<>裡面要填你原先要安裝的套件
```

點進去info會看到剛生成的檔案，這時候要把info資料夾內的內容全都搬到剛建好的info.bak裡面
```
sudo mv info/* info.bak
```

移完就可以把info刪除了
```
sudo rm -rf info
```

最後不能忘記info.bak要改回正港的info
```
sudo mv info.bak info
```