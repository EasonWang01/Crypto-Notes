# Digital Signature Algorithm {#firstHeading}

# \# 先瞭解的概念

1.

```
數位加密：用公鑰加密，只有用私鑰解開，因為私鑰只有你自己有，所以他保證了數據不能被別人看到
數位簽名：用私鑰加密，只能用公鑰解密，任何人都可以用公鑰驗證。因為私鑰只有你自己有，所以它可以保證數據只能是你發出的，不可能有別人發出，除非你得私鑰丟失或被第三方破解出來
```

2.

```
RSA 與 DSA 都是非對稱加密算法。其中RSA的安全性是基於極其困難的大整數的分解（兩個質數的乘積）；DSA 的安全性是基於整數有限域離散對數難題。基本上可以認為相同密鑰長度的 RSA 算法與 DSA 算法安全性相當。
```

3.

```
DSA 只能用於數字簽名，而無法用於加密（某些擴展可以支持加密）；RSA 即可作為數字簽名，也可以作為加密算法
```

# \#簽章過程:\(使用Openssl\)

> 查看相關指令

```
openssl gendsa
```

1. 產生一個1024bits的參數檔案

```
openssl dsaparam -out dsa_param.pem 1024
```

![](/assets/dsa01.png)

2.從剛才的參數檔案產生一個private key 並且用AES-128之password protect  也可用aes-192或aes-256

```
openssl gendsa -out dsa_privatekey.pem -aes128 dsa_param.pem
```

![](/assets/dsa02.png)

> PS:unable to write ''random state" 可參考此
>
> [https://stackoverflow.com/questions/94445/using-openssl-what-does-unable-to-write-random-state-mean](https://stackoverflow.com/questions/94445/using-openssl-what-does-unable-to-write-random-state-mean)
>
> 但其不影響

3.接著再從私鑰產生一把公鑰

```
openssl dsa -in dsa_privatekey.pem -pubout -out dsa_publickey.pem
```

4.之後我們新增一個我們要用來簽章的文件 ，並且在裡面寫一點字

```
vim document.txt
```

然後用私鑰對文件簽章產生一個sig

```
openssl dgst -dss1 -sign dsa_privatekey.pem -out document.sig document.txt
```

5.最後用公鑰驗證

```
openssl dgst -dss1 -verify dsa_publickey.pem -signature document.sig document.txt
```

![](/assets/dsa04.png)

## \#使用Node.js

先參考此文，產生pem

[https://adam.merrifield.ca/2012/01/20/generating-dsa-pem-key-pairs/](https://adam.merrifield.ca/2012/01/20/generating-dsa-pem-key-pairs/)

```js
var fs = require('fs');
var crypto = require('crypto');
var pem = require('pem')

var privateKey = fs.readFileSync('./dsa_priv0.pem');     
var publicKey = fs.readFileSync('./dsa_pub.pem');
var buffLen = 128;

var sign = crypto.createSign('dsaWithSHA1');
sign.update('apple');

// 注意 這裡是用私鑰 簽名   如果公鑰會出現錯誤
var res = sign.sign(privateKey, 'hex');

var verify = crypto.createVerify('dsaWithSHA1');
verify.update('apple');

var rst = verify.verify(publicKey, res, "hex");

console.log(rst);  // Prints success. means the key pair works.
```



