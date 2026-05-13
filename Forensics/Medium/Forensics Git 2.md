# Forensics Git 2
## 概要
分析磁碟映像檔（Disk Image）中的資料，尋找特定資料所在位置並開啟。

## 使用工具
* The Sleuth Kit (TSK): fls, icat, mmls
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
開啟home、ctf-player、Code、 killer-chat-app、logs後，發現有一堆沒用的txt檔
發現flag的commit hash: 177789af0b300e043ea8f54ea57d6cee352291ae

### 5.掛載磁碟
根據提示，flag應該藏在.git的紀錄內
```Bash
mkdir /tmp/storage #創資料夾
fdisk -l disk.img #檢查sector換算bytes的offset
sudo mount -o loop,offset=584056832 disk.img /tmp/storage #掛載
cd /tmp/storage
cd home
cd ctf-player
cd Code
cd  killer-chat-app #進入到.git資料夾所在的位置
find .git/objects -type f | sed 's|/||g; s|.gitobjects||g' | xargs -n 1 git cat-file -p | grep -aE "picoCTF|flag"
#1.在指定資料夾找出所有檔案。2.刪除路徑中的所有斜線，刪除字串中的 .gitobjects 部分。
#3.把前一階段的hash一個個執行，自動偵測類型後解壓縮。4.把二進位當作文字處理，同時搜尋多個關鍵字。
```
