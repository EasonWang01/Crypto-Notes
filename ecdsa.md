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

# ECC \(Elliptic curve cryptography\)

列出可用OpenSSL可用曲線

```
openssl ecparam -list_curves
```

以下網站可看到各曲線的Base  Point

[https://safecurves.cr.yp.to/base.html](https://safecurves.cr.yp.to/base.html)

曲線方程式 Ep\(a, b\) 為以下型態:

```
y**2 = x**3 + ax + b (mod p)
```

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

\(以下範例需使用python2\)

```python
#coding=utf-8

Pcurve = 2**256 - 2**32 - 2**9 - 2**8 - 2**7 - 2**6 - 2**4 -1 # The proven prime
N=0xFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFEBAAEDCE6AF48A03BBFD25E8CD0364141 # Number of points in the field
Acurve = 0; Bcurve = 7 # This defines the curve. y^2 = x^3 + Acurve * x + Bcurve
Gx = 55066263022277343669578718895168534326250603453777594175500187360389116729240
Gy = 32670510020758816978083085130507043184471273380659243275938904335757337482424
GPoint = (Gx,Gy) # This is our generator point. Tillions of dif ones possible

#Individual Transaction/Personal Information
privKey = 75263518707598184987916378021939673586055614731957507592904438851787542395619 #replace with any private key
RandNum = 28695618543805844332113829720373285210420739438570883203839696518176414791234 #replace with a truly random number
HashOfThingToSign = 86032112319101611046176971828093669637772856272773459297323797145286374828050 # the hash of your message/transaction

def modinv(a,n=Pcurve): #Extended Euclidean Algorithm/'division' in elliptic curves
    lm, hm = 1,0
    low, high = a%n,n
    while low > 1:
        ratio = high/low
        nm, new = hm-lm*ratio, high-low*ratio
        lm, low, hm, high = nm, new, lm, low
    return lm % n

def ECadd(xp,yp,xq,yq): # Not true addition, invented for EC. It adds Point-P with Point-Q.
    m = ((yq-yp) * modinv(xq-xp,Pcurve)) % Pcurve
    xr = (m*m-xp-xq) % Pcurve
    yr = (m*(xp-xr)-yp) % Pcurve
    return (xr,yr)

def ECdouble(xp,yp): # EC point doubling,  invented for EC. It doubles Point-P.
    LamNumer = 3*xp*xp+Acurve
    LamDenom = 2*yp
    Lam = (LamNumer * modinv(LamDenom,Pcurve)) % Pcurve
    xr = (Lam*Lam-2*xp) % Pcurve
    yr = (Lam*(xp-xr)-yp) % Pcurve
    return (xr,yr)

def EccMultiply(xs,ys,Scalar): # Double & add. EC Multiplication, Not true multiplication
    if Scalar == 0 or Scalar >= N: raise Exception("Invalid Scalar/Private Key")
    ScalarBin = str(bin(Scalar))[2:]
    Qx,Qy=xs,ys
    for i in range (1, len(ScalarBin)): # This is invented EC multiplication.
        Qx,Qy=ECdouble(Qx,Qy); # print "DUB", Qx; print
        if ScalarBin[i] == "1":
            Qx,Qy=ECadd(Qx,Qy,xs,ys); # print "ADD", Qx; print
    return (Qx,Qy)

print; print "******* Public Key Generation *********"
xPublicKey, yPublicKey = EccMultiply(Gx,Gy,privKey)
print "the private key (in base 10 format):"; print privKey; print
print "the uncompressed public key (starts with '04' & is not the public address):"; print "04",xPublicKey,yPublicKey

print; print "******* Signature Generation *********"
xRandSignPoint, yRandSignPoint = EccMultiply(Gx,Gy,RandNum)
r = xRandSignPoint % N; print "r =", r
s = ((HashOfThingToSign + r*privKey)*(modinv(RandNum,N))) % N; print "s =", s

print; print "******* Signature Verification *********>>"
w = modinv(s,N)
xu1, yu1 = EccMultiply(Gx,Gy,(HashOfThingToSign * w)%N)
xu2, yu2 = EccMultiply(xPublicKey,yPublicKey,(r*w)%N)
x,y = ECadd(xu1,yu1,xu2,yu2)
print r==x; print
```



# 另一個範例使用jsrsasign模組

https://www.npmjs.com/package/jsrsasign



```js
var r = require('jsrsasign');
var ec = new r.ECDSA({ 'curve': 'secp256r1' });
var keypair = ec.generateKeyPairHex();
var pubhex = keypair.ecpubhex; // hexadecimal string of EC public key
var prvhex = keypair.ecprvhex; // hexadecimal string of EC private key (=d)

console.log(pubhex)
console.log(prvhex)

msg1 = 123;

var sig = new r.Signature({ "alg": 'SHA256withECDSA' });
sig.init({ d: prvhex, curve: 'secp256r1' });
sig.updateString(msg1);
var sigValueHex = sig.sign();

var sig = new r.Signature({ "alg": 'SHA256withECDSA' });
sig.init({ xy: pubhex, curve: 'secp256r1' });
sig.updateString(msg1);
var result = sig.verify(sigValueHex);
if (result) {
  console.log("valid ECDSA signature");
} else {
  console.log("invalid ECDSA signature");
}

```



