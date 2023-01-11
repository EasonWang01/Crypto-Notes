# Big Endian & small Endian

```javascript
0x12345678：

Small Endian： 0x78 0x56 0x34 0x12 


Big Endian： 0x12 0x34 0x56 0x78
```

以下為範例程式（BigEndian\_to\_SmallEndian）

```javascript
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

如果要把Smail 轉為BIg Endian則為同一個程式，把數字再放入跑一次即可轉回

![](<assets/螢幕快照 2017-12-17 下午2.20.56.png>)
