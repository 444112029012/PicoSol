# Bitlocker-1

## 概要
解開bitlocker後，掛載該磁碟flag。

## 使用工具
* bitlocker2john、hashcat、rockyou.txt

## 方法 (Methodology)

### 1. 基礎環境準備
下載檔案

### 2. 提取雜湊值
  ```Bash
bitlocker2john -i bitlocker-1.dd > bitlocker_hash.txt
  ```
### 3. 設定暴力破解的密碼表
```Bash
head -n 10000 rockyou.txt > dic.txt
```
### 4. 解鎖
```Bash
hashcat -m 22100 bitlocker_hash.txt dic.txt -w 3
```
#### 記住解鎖密碼: jacqueline
### 5. 儲存 dislocker-file
```Bash
sudo dislocker -v bitlocker-1.dd -ujacqueline /tmp/storage
```
### 6. 重新掛載，cat flag.txt
```Bash
sudo ls -l /tmp/storage
sudo mount -o loop /tmp/storage/dislocker-file /tmp/storage/
cd tmp/storage
cat flag.txt
```
