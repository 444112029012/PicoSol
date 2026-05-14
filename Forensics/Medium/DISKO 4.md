# DISKO 4

## 概要
.dd檔找出flag。

## 使用工具
* grep, fdisk, binwalk

## 方法

### 1. 基礎環境準備
下載檔案並解壓縮

### 2. 檢查
  ```Bash
 fdisk -l disko-4.dd
  ```
### 3. 分離檔案
沒分區，且根據提示，需要查看刪除的檔案
! 直接icat innode編號會出錯
  ```Bash
 fls -r -d  disko-4.dd
binwalk -e disko-4.dd
  ```
查看資料
```Bash
cd _disko-4.dd.extracted
grep -r picoCTF
```
#### output: dont-delete:picoCTF{d3l_d0n7_h1d3_w3ll_c2fcb641}
