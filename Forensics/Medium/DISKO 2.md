# DISKO 2

## 概要
.dd檔找出flag。

## 使用工具
* grep, strings, dd, fdisk

## 方法

### 1. 基礎環境準備
下載檔案並解壓縮

### 2. 檢查
  ```Bash
 fdisk -l disko-2.dd
  ```
### 3. 搜尋
  ```Bash
 strings disko-2.dd | grep picoCTF
  ```
出現多個picoCTF，切割分區
```Bash
dd if=disko-2.dd of=disk.img bs=512 skip=2048 count=51200
strings disk.img | grep picoCTF
```
#### output: picoCTF{4_P4Rt_1t_i5_a93c3ba0}
