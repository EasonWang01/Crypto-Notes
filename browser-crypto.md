# Crypto module

## Argon2

[https://github.com/ranisalt/node-argon2](https://github.com/ranisalt/node-argon2)

## Bcrypt

[https://github.com/kelektiv/node.bcrypt.js](https://github.com/kelektiv/node.bcrypt.js)

## sjcl (By standford)

[https://github.com/bitwiseshiftleft/sjcl/tree/3668e639bc78e910815f501d55458e968845edc2](https://github.com/bitwiseshiftleft/sjcl/tree/3668e639bc78e910815f501d55458e968845edc2)

## PyCrypto (python crypto module)

[https://pycryptodome.readthedocs.io/en/latest/src/introduction.html](https://pycryptodome.readthedocs.io/en/latest/src/introduction.html)

## **Web Crypto API**

[https://www.w3.org/TR/WebCryptoAPI/#crypto-interface](https://www.w3.org/TR/WebCryptoAPI/#crypto-interface)

[https://developer.mozilla.org/en-US/docs/Web/API/Crypto](https://developer.mozilla.org/en-US/docs/Web/API/Crypto)

[https://developer.mozilla.org/en-US/docs/Web/API/SubtleCrypto/digest](https://developer.mozilla.org/en-US/docs/Web/API/SubtleCrypto/digest)

```javascript
async function sha256(message) {
    const msgBuffer = new TextEncoder('utf-8').encode(message);                     // encode as UTF-8
    const hashBuffer = await crypto.subtle.digest('SHA-256', msgBuffer);            // hash the message
    const hashArray = Array.from(new Uint8Array(hashBuffer));                       // convert ArrayBuffer to Array
    const hashHex = hashArray.map(b => ('00' + b.toString(16)).slice(-2)).join(''); // convert bytes to hex string
    return hashHex;
}

sha256('abc').then(hash => console.log(hash));

(async function() {
    const hash = await sha256('abc');
}());
```
