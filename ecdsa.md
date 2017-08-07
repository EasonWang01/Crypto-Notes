# 

有三個類似名詞為**ECC、ECDH、ECDSA，第一個是**Elliptic Curve Cryptography的縮寫，而後面兩個都是基於ECC的加密演算法

```
（1）相同密鑰長度下，安全性能更高，如160位ECC已經與1024位RSA、DSA有相同的安全強度。
（2）計算量小，處理速度快，在私鑰的處理速度上（解密和簽名），ECC遠 比RSA、DSA快得多。
（3）存儲空間占用小 ECC的密鑰尺寸和系統參數與RSA、DSA相比要小得多， 所以占用的存儲空間小得多。
```

```
 ECDH和ECDSA產生公私鑰的方式都相同
```

# ECC

列出可用OpenSSL可用曲線

```
openssl ecparam -list_curves
```

以下網站可看到各曲線的Base  Point

https://safecurves.cr.yp.to/base.html

# 

# ECDH

用途:

```
讓兩人不透漏私密訊息的情況下獲得一把共同的密鑰
```

ECDH可視為ECC + DH \(Diffie-Hellman\)

Ex:

```js
var crypto = require('crypto');

// 1.
var ecdh = crypto.createECDH('secp256k1');
ecdh.generateKeys();
var publicKey = ecdh.getPublicKey(null, 'compressed');

// 2.
var ecdh2 = crypto.createECDH('secp256k1');
ecdh2.generateKeys();
var publicKey2 = ecdh2.getPublicKey(null, 'compressed');

// 3.
var ecdh3 = crypto.createECDH('secp256k1');
ecdh3.generateKeys();
var publicKey3 = ecdh3.getPublicKey(null, 'compressed');

// 4.
var ecdh4 = crypto.createECDH('secp256k1');
ecdh4.generateKeys();
var publicKey4 = ecdh4.getPublicKey(null, 'compressed');


//以下倆倆為一組，因用A之ecdh與B之public key 算出之結果與用 B之ecdh 與A之public key 相同
var secret = ecdh.computeSecret(publicKey2);
console.log('Secret1: ', secret.length, secret.toString('hex'));

var secret2 = ecdh2.computeSecret(publicKey);
console.log('Secret2: ', secret2.length, secret2.toString('hex'));

var secret3 = ecdh3.computeSecret(publicKey4);
console.log('Secret2: ', secret3.length, secret3.toString('hex'));

var secret4 = ecdh4.computeSecret(publicKey3);
console.log('Secret2: ', secret4.length, secret4.toString('hex'));
```

結果如下:

&gt; 以下倆倆為一組，因用A之ecdh與B之public key 算出之結果與用 B之ecdh 與A之public key 相同

![](/assets/4523.png)

> 但你可能會問兩個人算出共同密鑰可以做什麼?

通常過程是:兩人只要把自己的public key給對方來算出secret key，之後用這把secret去做AES之類加密，即可在不傳遞真正密鑰的情況下只有兩人可以解密

原理:

![](/assets/7458.png)

```
小明與阿東 兩人協議好要使用 p=23以及 g=5.

小明選擇一個秘密整數a=6, 計算A = (g ** a) % p 然後傳給給阿東。
A = (5 ** 6) % 23 = 8.

阿東選擇一個秘密整數b=15, 計算B = (g ** b) % p 然後傳給小明。
B = (5 ** 15) % 23 = 19.

小明計算s = (B ** a) % p
(19 ** 6) % 23 = 2.
阿東計算s = (A ** b) % p
(8 ** 15) % 23 = 2.
```

# ECDSA（The Elliptic Curve Digital Signature Algorithm）

ECDSA也可視為ECC+DSA\(Digital Signature Algorithm\)

視覺化網站:[https://cdn.rawgit.com/andreacorbellini/ecc/920b29a/interactive/reals-add.html](https://cdn.rawgit.com/andreacorbellini/ecc/920b29a/interactive/reals-add.html)

常見用於:[TLS](https://tools.ietf.org/html/rfc4492)   [PGP](https://tools.ietf.org/html/rfc6637)  [SSH](https://tools.ietf.org/html/rfc5656) 和部分加密貨幣

---



