# Forensics Git 0
## 概要
分析磁碟映像檔（Disk Image）中的資料，尋找特定資料所在位置並開啟。

## 使用工具
* The Sleuth Kit (TSK): fls, icat
* Linux Command Line: gunzip

## 方法 (Methodology)

### 1. 基礎環境準備
首先將下載的壓縮檔解壓縮，取得原始磁碟映像檔。

```Bash
gunzip disk.img.gz
```
### 2. 找出系統分區起點

```Bash
mmls disk.img
```

### 3. 找尋奇怪分區
Linux系統分區出現二次，推測第二次有問題，查看該分區資料夾
```Bash
fls -o 1140736 disk.img
```
### 4. 一層層開啟
開啟home、ctf-player、Code、secrets、.git、logs、HEAD後即可找到(最後位置):
```Bash
icat -o 1140736 disk.img 65692
```

#### 輸出: Wrap this phrase in the flag format: g17_1n_7h3_d15k_041217d8
