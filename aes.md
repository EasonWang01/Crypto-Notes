# AES

對稱式加密，加解密用同一個密碼或密鑰，主要為DES的改進版

EX:

```js
const crypto = require('crypto');
const password = 'mentos asert asood';
const cipher = crypto.createCipher('aes192', password);


let encrypted = cipher.update('I have a pig', 'utf8', 'hex');
encrypted += cipher.final('hex');
console.log('---加密後的資訊為---')
console.log(encrypted);


const decipher = crypto.createDecipher('aes192', password);
let decrypted = decipher.update(encrypted, 'hex', 'utf8');
decrypted += decipher.final('utf8');
console.log('---解密後的資訊為---')
console.log(decrypted);
```





