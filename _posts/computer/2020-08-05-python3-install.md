
### 從pypa.io下載安裝指令碼
安裝pip之前必須安裝Python。  
Ubuntu在13.04之後的版本都有預載python3了，確認一下是否真的有安裝。
```
python3 --version
```
確認過後真的有，接著以下步驟。這個指令說是從pypa.io下載安裝最新版本的pip和setuptools的套件。
```
curl -O https://bootstrap.pypa.io/get-pip.py
```
* 小註解：
  curl是Linux上透過HTTP協定下載和上傳檔案的指令，格式： `curl [options] [url...]`
  `-o` 搭配欲下載檔名和網址
  `-O` 直接使用下載網址的檔名來命名下載的檔案

### 使用Python執行
使用python3直接呼叫可以確保即使系統存在較早版本，pip仍然會安裝於適當位置。
```
python3 get-pip.py --user
```

### 新增PATH變數
確認擁有哪個shell
```
echo $SHELL
```
在使用者資料夾中尋找shell的描述檔指令碼
```
ls -a
```
用vim更改
```
vim ~/.bash_profile
```
進入檔案之後增加
``` bash
export PATH=$PATH:$HOME/.local/bin/
```
一般來說重開機就可以生效，要立刻讓檔案設定生效，可以執行指令：
```
source ~/.bash_profile
```

### 確認已經正確安裝pip
```
pip ---version
```

---
## 參考資料
[在 Linux 上安裝 Python、pip 和 EB CLI](https://docs.aws.amazon.com/zh_tw/elasticbeanstalk/latest/dg/eb-cli3-install-linux.html)
[Linux Curl Command 指令與基本操作入門教學](https://blog.techbridge.cc/2019/02/01/linux-curl-command-tutorial/)