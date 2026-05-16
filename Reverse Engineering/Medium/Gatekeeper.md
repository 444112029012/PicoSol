# Gatekeeper

## 概要
透過已知程式碼運作找到key，再藉由key找明文。

## 使用工具
* r2
* cyberchef: Replace, Reverse

## 方法 (Methodology)

### 1. 基礎環境準備
下載檔案

### 2. 查看檔案
使用 r2 (radare2) 觀看main結構
```Bash
r2 ./gatekeeper
```
```Bash
aaa //分析檔案
s main 移動到main的位址
pdf 列出組合語言
```
### 3. 分析main
發現密碼規則:
1. 大於999(0x3e7)
2. 小於9999(0x270f)
3. 長度要等於3 ([var_34h], 3)
發現輸入可以轉換16進制
```Bash
call sym.imp.strtol         ; long strtol(const char *str, char * *endptr, int base)
```
### 4. 解題
連線官方位址後，輸入16進位大於999的值(如:fff)
#### key: }3f3ftc_oc_ip1ac6ftc_oc_ip4_99ftc_oc_ip9_TGftc_oc_ip_xehftc_oc_ip_tigftc_oc_ipid_3ftc_oc_ip{FTCftc_oc_ipocipftc_oc_ip

### 5. 觀察reveal_flag
```Bash
[0x00001280]> s sym.reveal_flag
[0x00001449]> pdf
```
發現會重複輸出ftc_oc_ip，且反向輸出

### 6.解密文
cyberchef 使用replace(ftc_oc_ip), 接上reverse即可得到結果
