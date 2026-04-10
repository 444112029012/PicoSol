# Timeline1

## 概要
分析磁碟映像檔（Disk Image）中的資料，透過還原檔案系統時間線定位異常變動，最終在偽裝的系統設定檔中找出 Flag。

## 使用工具
* The Sleuth Kit (TSK): fls, mactime, icat
* Linux Command Line: gunzip, grep, tail, base64

## 方法 (Methodology)

### 1. 基礎環境準備
首先將下載的壓縮檔解壓縮，取得原始磁碟映像檔。

```Bash
gunzip disk.img.gz
```
### 2. 建立檔案系統時間線 (MAC Timeline)
為了還原系統關機前的最後行為，使用 TSK 工具建立時間線：
* 步驟 A: 建立 Body File (包含所有元數據)
  ```Bash
  fls -r -m / partition4.img > body.txt
  ```
* 步驟 B: 轉換為人類可讀的時間線格式
  ```Bash
  mactime -b body.txt > timeline.txt
  ```
### 3. 過濾關鍵變動 (MACB Filter)
根據提示，我們關注系統關機前的最後動作。利用 grep 過濾出屬性為 macb (Modified, Accessed, Changed, Birth) 的新建立檔案，並查看最後 20 筆記錄：
```Bash
grep "macb" timeline.txt | tail -n 20
```
### 4. 調查可疑線索
* 分析 .ash_history: 透過 Inode 4943 查詢，發現內容僅為 poweroff，判斷原始指令已被清理。
  指令: icat partition4.img 4943
* 鎖定異常檔案: 在 macb 清單中發現 /etc/chat (Inode 32716)，體積僅 49 bytes，且在關機前最後一秒被建立，極度可疑。

### 5. 提取與解碼 (Final Extraction)
使用 icat 提取該 Inode 的原始 Base64 字串並解碼：
```Bash
icat partition4.img 32716
```
#### 輸出: NTczNDE3aDEzcl83aDRuXzdoM18xNDU3XzU4NTI3YmIyMjIK

```Bash
echo "NTczNDE3aDEzcl83aDRuXzdoM18xNDU3XzU4NTI3YmIyMjIK" | base64 -d
```
#### 輸出: 573417h13r_7h4n_7h3_1457_58527bb222

