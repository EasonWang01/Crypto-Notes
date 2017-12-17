```js
0x12345678：

Small Endian： 0x78 0x56 0x34 0x12 


Big Endian： 0x12 0x34 0x56 0x78
```

以下為範例程式（BigEndian\_to\_SmallEndian）

```js
function BigEndian_to_SmallEndian(hexNum) {
  let SmallEndian_array = [];
  if (hexNum.length % 2 !== 0) {
    hexNum = '0' + hexNum // 數字總數為奇數的話 在開頭補0
  }

  for (let i = 0, len = hexNum.length; i < len; i++) {
    if (i % 2 !== 0) {
      SmallEndian_array.unshift(hexNum.charAt(i - 1) + hexNum.charAt(i))
    }
  }

  return SmallEndian_array.join('');
}
```



