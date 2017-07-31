# ECDSA（The Elliptic Curve Digital Signature Algorithm）

視覺化網站:[https://cdn.rawgit.com/andreacorbellini/ecc/920b29a/interactive/reals-add.html](https://cdn.rawgit.com/andreacorbellini/ecc/920b29a/interactive/reals-add.html)

常見用於:[TLS](https://tools.ietf.org/html/rfc4492)   [PGP](https://tools.ietf.org/html/rfc6637)  [SSH](https://tools.ietf.org/html/rfc5656) 和部分加密貨幣

有三個類似名詞為**ECC、ECDH、ECDSA，第一個是**Elliptic Curve Cryptography的縮寫，而後面兩個都是基於ECC的加密演算法

而ECDSA也可視為ECC+DSA\(Digital Signature Algorithm\)



```
（1）相同密鑰長度下，安全性能更高，如160位ECC已經與1024位RSA、DSA有相同的安全強度。
（2）計算量小，處理速度快，在私鑰的處理速度上（解密和簽名），ECC遠 比RSA、DSA快得多。
（3）存儲空間占用小 ECC的密鑰尺寸和系統參數與RSA、DSA相比要小得多， 所以占用的存儲空間小得多。
```



