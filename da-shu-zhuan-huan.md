# **大數BigInteger 進位轉換**

**\(由於JS有MAX\_SAFE\_INTEGER\)所以大數字需要用以下方法轉換進位，python內建BigInteger所以可以直接用int\(\),hex\(\)等直接進行轉換計算**

[https://developer.mozilla.org/zh-TW/docs/Web/JavaScript/Reference/Global\_Objects/Number/MAX\_SAFE\_INTEGER](https://developer.mozilla.org/zh-TW/docs/Web/JavaScript/Reference/Global_Objects/Number/MAX_SAFE_INTEGER)

**所以有時會失去精度\(後面幾位數都變為0\)**

**ex:\(36028797018963970\).toString\(16\) 但Python內建BIgInteger 所以出來數字會跟JS不同**

**而JS要解決這個問題要使用函數先將數字分開轉換再合併**

> Big-integer模組只能對十進位計算有效，其他進位也會失去精準度





例如以下問題



```
parseInt("10001111111111000001111111111111111111111111111111111111111000000000000111111111111111111111", 2)

//會直接出現以下數字，佔有了科學記號這樣在之後轉換會失真

2.7850723016701677e+27
```





# 1.二進位轉十進位

```js
function bin2dec(str){ 
    var dec = str.toString().split(''), sum = [], hex = [], i, s
    while(dec.length){
        s = 1 * dec.shift()
        for(i = 0; s || i < sum.length; i++){
            s += (sum[i] || 0) * 2
            sum[i] = s % 10
            s = (s - sum[i]) / 10
        }
    }
    while(sum.length){
        hex.push(sum.pop().toString(10))
    }
    return hex.join('')
}
```



所以我們改用上面的方法

```
bin2dec("10001111111111000001111111111111111111111111111111111111111000000000000111111111111111111111")

就可以成功在JS中執行大數二進位轉換
"2785072301670167691931942911"
```

# 

# 二進位轉十六進位

```js
function binToHex(str){
var dec = str.toString().split(''), sum = [], hex = [], i, s
while(dec.length){
  s = 1 * dec.shift();
  for(i = 0; s || i < sum.length; i++){
    s += (sum[i] || 0) * 2
    sum[i] = s % 16
    s = (s - sum[i]) / 16
  }
}
  while(sum.length){
    hex.push(sum.pop().toString(16))
  }
  return hex.join('')
}
```

# 2.十六進位轉二進位

```js
var lookup = {
  '0': '0000',
  '1': '0001',
  '2': '0010',
  '3': '0011',
  '4': '0100',
  '5': '0101',
  '6': '0110',
  '7': '0111',
  '8': '1000',
  '9': '1001',
  'a': '1010',
  'b': '1011',
  'c': '1100',
  'd': '1101',
  'e': '1110',
  'f': '1111',
  'A': '1010',
  'B': '1011',
  'C': '1100',
  'D': '1101',
  'E': '1110',
  'F': '1111'
};

function hexToBinary(s) {
    var ret = '';
    for (var i = 0, len = s.length; i < len; i++) {
        ret += lookup[s[i]];
    }
    return ret;
}
```

# 十六進位轉十進位

```js
function hexToDecimal(s) {

    function add(x, y) {
        var c = 0, r = [];
        var x = x.split('').map(Number);
        var y = y.split('').map(Number);
        while(x.length || y.length) {
            var s = (x.pop() || 0) + (y.pop() || 0) + c;
            r.unshift(s < 10 ? s : s - 10); 
            c = s < 10 ? 0 : 1;
        }
        if(c) r.unshift(c);
        return r.join('');
    }

    var dec = '0';
    s.split('').forEach(function(chr) {
        var n = parseInt(chr, 16);
        for(var t = 8; t; t >>= 1) {
            dec = add(dec, dec);
            if(n & t) dec = add(dec, '1');
        }
    });
    return dec;
}
```



