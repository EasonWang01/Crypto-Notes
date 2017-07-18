# \#[SubtleCrypto](https://developer.mozilla.org/en-US/docs/Web/API/SubtleCrypto)

[https://developer.mozilla.org/en-US/docs/Web/API/SubtleCrypto](https://developer.mozilla.org/en-US/docs/Web/API/SubtleCrypto)

# \#[sjcl](https://github.com/bitwiseshiftleft/sjcl/tree/3668e639bc78e910815f501d55458e968845edc2) \(By standford\)

[https://github.com/bitwiseshiftleft/sjcl/tree/3668e639bc78e910815f501d55458e968845edc2](https://github.com/bitwiseshiftleft/sjcl/tree/3668e639bc78e910815f501d55458e968845edc2)

# \#[**Web Crypto API**](https://developer.mozilla.org/en-US/docs/Web/API/Web_Crypto_API)

[https://www.w3.org/TR/WebCryptoAPI/\#crypto-interface](https://www.w3.org/TR/WebCryptoAPI/#crypto-interface)

[https://developer.mozilla.org/en-US/docs/Web/API/Crypto](https://developer.mozilla.org/en-US/docs/Web/API/Crypto)



EX:\(from MDN\)

```js
function sha256(str) {
  // We transform the string into an arraybuffer.
  var buffer = new TextEncoder("utf-8").encode(str);
  return crypto.subtle.digest("SHA-256", buffer).then(function (hash) {
    return hex(hash);
  });
}

function hex(buffer) {
  var hexCodes = [];
  var view = new DataView(buffer);
  for (var i = 0; i < view.byteLength; i += 4) {
    // Using getUint32 reduces the number of iterations needed (we process 4 bytes each time)
    var value = view.getUint32(i)
    // toString(16) will give the hex representation of the number without padding
    var stringValue = value.toString(16)
    // We use concatenation and slice for padding
    var padding = '00000000'
    var paddedValue = (padding + stringValue).slice(-padding.length)
    hexCodes.push(paddedValue);
  }

  // Join all the hex strings into one
  return hexCodes.join("");
}

sha256("abc").then(function(digest) {
  console.log(digest);
});
```



