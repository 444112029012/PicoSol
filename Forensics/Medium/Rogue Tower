# Rogue Tower

## 概要
分析一個PCAP檔，找到異常封包，再根據這封包找出嘗試連線的設備，並找出flag。

## 使用工具
* tshark
* cyberchef: HEX, Base64, XOR

## 方法 (Methodology)

### 1. 基礎環境準備
下載PCAP檔

### 2. 查看封包

```Bash
tshark -r rogue_tower.pcap
```

### 3. 根據提示從udp55000的封包下手
```Bash
tshark -r rogue_tower.pcap -Y 'udp.port == 55000'
```
### 4. 調查可疑線索
發現只有二個ip出現，其中192.168.99.1看起來很奇怪

### 5. 再次確認
使用 -x 提查看封包內容，發現包含 unauthorized 的訊息：
```Bash
tshark -r rogue_tower.pcap -Y 'udp.port == 55000' -x
```

### 6. 觀察其他封包
找到此封包全域廣播後的其他封包行為，發現10.100.50.122開始發送訊息：
```Bash
tshark -r rogue_tower.pcap -Y "frame.number >= 14" | head -n 20
```
### 7. 找密文
把這些封包的資料拿出來
```Bash
tshark -r rogue_tower.pcap -Y 'ip.addr == 10.100.50.122 && http.request.method =="POST"' -T fields -e http.file_data
```

#### 輸出: 51464657576e5a6a666b784343464a41426d686242467855616b45465141744662317858566745484141514252513d3d

### 8. 找金鑰
根據提示尋找IMSI的編號
```Bash
tshark -r rogue_tower.pcap -Y "frame.number == 16" -T fields -e http.user_agent
```

#### 輸出: IMSI:310410308555787

將此密文放到cyberchef先hex、接著base64、發現是亂碼，再對此亂碼用金鑰做XOR運算，不斷刪掉金鑰第一位，直到明文出來是正確的pico形式
