# DISKO 3

## 概要
.dd檔找出flag。

## 使用工具
* grep, fdisk

## 方法

### 1. 基礎環境準備
下載檔案並解壓縮

### 2. 檢查
  ```Bash
 fdisk -l disko-3.dd
  ```
### 3. 掛載
因為沒顯示分區，所以整個掛載
  ```Bash
 sudo mount -o loop disko-3.dd /tmp/storage
  ```
查看資料
```Bash
sudo ls /tmp/storage/log
sudo gunzip /tmp/storage/log/flag.gz
sudo cat /tmp/storage/log/flag
```
#### output: picoCTF{n3v3r_z1p_2_h1d3_654235e0}
