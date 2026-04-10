# Timeline0

## 概要
分析磁碟映像檔（Disk Image）中的資料，透過檔案系統時間線定位異常，在系統設定檔中找出 Flag。

## 使用工具
* The Sleuth Kit (TSK): fls, mactime, icat
* Linux Command Line: gunzip, grep, tail, base64

## 方法 (Methodology)

### 1. 基礎環境準備
首先將下載的壓縮檔解壓縮，取得原始磁碟映像檔。

```Bash
gunzip partition4.img.gz
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
### 3. 查看檔案
根據提示，查看檔案中之時間線：
```Bash
less timeline.txt
```
### 4. 調查可疑線索
發現一個時間線很早的檔案(1985)
```Bash
grep 1985 timeline.txt
```

### 5. 提取與解碼 (Final Extraction)
使用 icat 提取該 Inode 的原始 Base64 字串並解碼：
```Bash
icat partition4.img 4945
```
#### 輸出: NzFtMzExbjNfMHU3MTEzcl9oM3JfNDNhMmU3YWYK

```Bash
echo "NzFtMzExbjNfMHU3MTEzcl9oM3JfNDNhMmU3YWYK" | base64 -d
```
#### 輸出: 71m311n3_0u7113r_h3r_43a2e7af
