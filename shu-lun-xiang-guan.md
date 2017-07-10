# 找出質數

&gt; 原理:不斷加2，然後把前面找到的質數也除看看\(因為質數不會與其前面任何質數相除為0\)，最後再把2放到開頭

```
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



