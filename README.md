eckey
=====

JavaScript component for Elliptical curve cryptography for crypto currencies such as Bitcoin, Litecoin, Dogecoin, etc.


Why?
----

This module provides a convenient way to compute relevant crypto currency operations that adhere to elliptical curve cryptography. To
really understand how private keys, public keys, addresses, and how elliptical curve cryptography works with JavaScript, read this: http://procbits.com/2013/08/27/generating-a-bitcoin-address-with-javascript


Installation
------------

    npm install --save eckey


Usage
-----

### API

#### ECKey([bytes], [compressed])

Constructor function.

- **bytes**: The private key bytes. Must be 32 bytes in length. Should be an `Array`, `Uint8Array`, or a `Buffer`.
- **compressed**: Specify whether the key should be compressed or not.

```js
var ECKey = require('eckey');
var secureRandom = require('secure-random'); 

var bytes = secureRandom(32); //https://github.com/jprichardson/secure-random
var key1 = new ECKey(bytes);
var key2 = ECKey(bytes); //<--- can also use without "new"
var compressedKey = new ECKey(bytes, true);
```

Note: Previous versions of this module would generate a random array of bytes for you if you didn't pass as input any to the constructor. This behavior has been removed to remove complexity and to ensure that the random generation is done securely. In the past, it wasn't.


#### compressed

Get/set whether the point on the curve is compressed. Affects the output of the WIF (wallet import format) and the address.


#### privateKey

Get/set the private key. When setting, the type can be either an `Array`, `Buffer`, or `Uint8Array`. When getting, the type is always `Buffer`. Setting would be useful if you don't pass a private key to the constructor.

```js
var ECKey = require('eckey');
var conv = require('binstring');

var privateKeyHex = "1184cd2cdd640ca42cfc3a091c51d549b2f016d454b2774019c2b2d2e08529fd";

var key = new ECKey(conv(privateKeyHex, {in: 'hex', out: 'buffer'}), false);
console.log(key.privatekey.toString('hex')); // => 1184cd2cdd640ca42cfc3a091c51d549b2f016d454b2774019c2b2d2e08529fd

var keyCompressed = ECKey(conv(privateKeyHex, {in: 'hex', out: 'buffer'}), true);

//nothing changes when compressed
console.log(key.privatekey.toString('hex')); // => 1184cd2cdd640ca42cfc3a091c51d549b2f016d454b2774019c2b2d2e08529fd
```


#### publicKey

Get the public key. The type is `Buffer`.

```js
var ECKey = require('eckey');
var conv = require('binstring');

var privateKeyHex = "1184cd2cdd640ca42cfc3a091c51d549b2f016d454b2774019c2b2d2e08529fd";

var key = new ECKey(conv(privateKeyHex, {in: 'hex', out: 'buffer'}), false);
console.log(key.publickey.toString('hex')); 
// => 04d0988bfa799f7d7ef9ab3de97ef481cd0f75d2367ad456607647edde665d6f6fbdd594388756a7beaf73b4822bc22d36e9bda7db82df2b8b623673eefc0b7495

var keyCompressed = ECKey(conv(privateKeyHex, {in: 'hex', out: 'buffer'}), true);

console.log(key.publickey.toString('hex')); 
// => 03d0988bfa799f7d7ef9ab3de97ef481cd0f75d2367ad456607647edde665d6f6f
```


### publicHash

Alias: `pubKeyHash`

Get the public hash i.e. the ripemd160(sha256(publicKey))

```js
var ECKey = require('eckey');
var conv = require('binstring');

var privateKeyHex = "1184cd2cdd640ca42cfc3a091c51d549b2f016d454b2774019c2b2d2e08529fd";

var key = new ECKey(conv(privateKeyHex, {in: 'hex', out: 'buffer'}), false);
console.log(key.publicHash.toString('hex')) // => 3c176e659bea0f29a3e9bf7880c112b1b31b4dc8
console.log(key.publKeyHash.toString('hex')) // => 3c176e659bea0f29a3e9bf7880c112b1b31b4dc8

var keyCompressed = ECKey(conv(privateKeyHex, {in: 'hex', out: 'buffer'}), true);
console.log(key.publicHash.toString('hex')) // => a1c2f92a9dacbd2991c3897724a93f338e44bdc1
console.log(key.publKeyHash.toString('hex')) // => a1c2f92a9dacbd2991c3897724a93f338e44bdc1
```



References
----------
- http://procbits.com/2013/08/27/generating-a-bitcoin-address-with-javascript
- https://github.com/bitcoinjs/bitcoinjs-lib/blob/master/src/eckey.js
- https://github.com/vbuterin/bitcoinjs-lib/blob/master/src/eckey.js
- 





