# Hidden Cipher 1

## 概要
透過已知程式碼運作找到key，再藉由key找明文。

## 使用工具
* ltrace
* cyberchef: HEX, XOR

## 方法 (Methodology)

### 1. 基礎環境準備
下載.zip檔並解壓縮

### 2. 查看檔案

```Bash
file hiddencipher
```

### 3. 執行檔案，在利用ltrace觀察檔案執行過程
```Bash
./hiddencipher
ltrace ./hiddencipher
```
### 4. 找出key
把加密後密文用hex解密，再跟flag.txt內的資料做XOR得key
####key: S3Cr3tS3Cr3tS3Cr3t

### 5. 遠端連線後取得密文
再把密文跟key做XOR得到flag
