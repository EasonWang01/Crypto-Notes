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

