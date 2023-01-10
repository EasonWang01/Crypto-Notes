---
description: OpenSSL 內建 Linux, Mac 等系統中，可以使用一些指令協助我們
---

# OpenSSL 實用指令

### 查看版本

```
openssl version -a
```

### 查看所有可用指令

```
openssl help
```

### 查看所有可以使用的指令標籤

```
openssl dgst -h
```

### 查看所有可用的加密演算法

```
openssl ciphers -v
```

### 測試加密執行速度

```
openssl speed
```

### 測試遠端 https 連線效率

```javascript
openssl s_time -connect remote.host:443 -www /test.html -new
```

### 產生自己簽發的證書

```javascript
openssl req \
  -x509 -nodes -days 365 -sha256 \
  -newkey rsa:2048 -keyout mycert.pem -out mycert.pem
```

### 下載其他網站的證書

```javascript
echo -n | openssl s_client -connect $HOST:$PORTNUMBER -servername $SERVERNAME \
    | openssl x509 > /tmp/$SERVERNAME.cert
```

{% embed url="https://serverfault.com/a/192731" %}

### 產生檔案的雜湊值

```javascript
# MD5 digest
openssl dgst -md5 filename

# SHA1 digest
openssl dgst -sha1 filename

# SHA256 digest
openssl dgst -sha256 filename
```

### 產生對應的 base64 編碼

```javascript
# 把 file.txt 轉 base64 後寫入到 terminal
openssl enc -base64 -in file.txt

# 寫入到 file.txt.enc
openssl enc -base64 -in file.txt -out file.txt.enc

# 直接在 terminal 輸出
$ echo "encode me" | openssl enc -base64
```

### 直接在 Terminal 用 AES 加密檔案

> 輸入後會要你打上密碼

```javascript
openssl enc -aes-256-cbc -salt -in test.txt -out file.enc
```

解密

```
openssl enc -d -aes-256-cbc -in file.enc
```

> 除了 AES 外可用以下指令查詢所有可用的加密算法
>
> ```
> openssl list-cipher-commands
> ```