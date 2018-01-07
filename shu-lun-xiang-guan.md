# 找出質數

> 原理:不斷加2，然後把前面找到的質數也除看看\\(因為質數不會與其前面任何質數相除為0\\)，最後再把2放到開頭

```js
function findPrime(num) {
  var primes = [];
  for (let n = 3; n <= num; n += 2) {
    if (primes.every(function (prime) { return n % prime != 0 })) {
      primes.push(n);
    }
  }
  primes.unshift(2);
  return primes
}
```

# 模反元素

> 整數 a 對模數 n 之模反元素b存在的充分必要條件是 a 和 n 互質

```
a x b 除與 n 餘數等於1

等同於下式

a x b ≡ 1 (mod n)
```

b即為a之模反元素

> 假設a 為 3 以及n 為 11 則算出b 為 4

# 中國剩餘定理

[https://www.youtube.com/watch?v=PM2D3xzqH\\_E&t=327s&list=LLeiE2pix0r2Mn7Xm4zi3WYg&index=7](https://www.youtube.com/watch?v=PM2D3xzqH_E&t=327s&list=LLeiE2pix0r2Mn7Xm4zi3WYg&index=7]%28https://www.youtube.com/watch?v=PM2D3xzqH_E&t=327s&list=LLeiE2pix0r2Mn7Xm4zi3WYg&index=7%29\)

由來:也常被稱韓信點兵，一個數除三餘2，除五餘3，除七餘2，求此數?

算法:

```
1.先將三和五相乘 = 15，15的倍數30，剛好除七餘2      記下30
2.再來將三和七相乘 = 21，21的倍數63，剛好除五餘3     記下63
3.最後將五和七相乘 = 35，而35剛好除三餘2           記下35
```

最後30+63+35 = 128 即為我們要找的數

# 歐拉函數

```
φ(a) 意思為小於a的所有數與a構成互質關係的個數
e.g. φ(5) = 4 (包含1,2,3,4)
```

而如果a可以被因式分解找出兩個數則φ\(a\)也等於如下

```
φ(a) = φ (c x d) = φ(c) x φ(d) 

// 所以假設今天 c 和 d 都是質數，在條件a = c * d下
則 φ(a) = (c - 1) * (d - 1) ， 因為c為質數下φ(c) = c - 1
```

> 其公式證明需用到中國剩餘定理

# 歐拉定理

假設有兩數a和b，且兩數互質

```
a ** φ(b) ≡ 1(mod b)
```

# 費馬小定理

今天如果歐拉定理中的b是質數則其變為費馬小定理

> 因為b為質數，所以小於他並與它互質的數之數量為b - 1\(因為小於質數的數都與他互值，所以φ\(b\)可以替換為 b - 1\)

假設有兩數a和b，a為整數，b為質數，且a不是b的倍數，則以下恆成立

```
(a ** b - 1) % b === 1
```

應用如下：

> 在下圖中第二到第三步驟時的2\*\*12可換為1**8 即是應用了此定理。\(因為2 \*\*** 13 - 1 % 13 = 1\)

![](/assets/螢幕快照 2018-01-06 上午9.20.00.png)

# 快速取模運算\(fast-modular-exponentiation\)

```
22 ** 2343 % 233
// 上式計算耗費資源，所以可以用公式，參考以下網站
```

[https://www.khanacademy.org/computing/computer-science/cryptography/modarithmetic/a/fast-modular-exponentiation](https://www.khanacademy.org/computing/computer-science/cryptography/modarithmetic/a/fast-modular-exponentiation)

```
1.將數字進行2的因數分解為二進位：https://tw.answers.yahoo.com/question/index?qid=20100629000010KK07683
2.然後參考下圖
3.然後運用5^2 mod 19 = (5^1 * 5^1) mod 19 = (5^1 mod 19 * 5^1 mod 19) mod 19 類似做法去分解成較小的數
```

![](/assets/85b4660da7c4e4f1e1662686a9771a51b2cf4d08ww.jpg)

## 大步小步算法

$$a^x ≡ b (mod p)$$假設今天是給定a,b,p要求x ，則可用大步小步算法



