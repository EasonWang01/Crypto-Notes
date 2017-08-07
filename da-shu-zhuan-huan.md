\# **大數BigInteger 進位轉換**

**\(由於JS有MAX\_SAFE\_INTEGER\)所以大數字需要用以下方法轉換進位**

[https://developer.mozilla.org/zh-TW/docs/Web/JavaScript/Reference/Global\_Objects/Number/MAX\_SAFE\_INTEGER](https://developer.mozilla.org/zh-TW/docs/Web/JavaScript/Reference/Global_Objects/Number/MAX_SAFE_INTEGER)

**所以有時會失去精度\(後面幾位數都變為0\)**

**ex:\(36028797018963970\).toString\(16\) 但Python內建BIgInteger 所以出來數字會跟JS不同**

**而JS要解決這個問題要使用函數先將數字分開轉換再合併**

16進位轉二進位

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



