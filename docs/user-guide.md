# User Guide

All information for developers using `fourtwozerojs-abi` should consult this document.

## Install

```
npm install --save fourtwozerojs-abi
```

## Usage

```js
const abi = require('fourtwozerojs-abi');
const SimpleStoreABI = [{"constant":false,"inputs":[{"name":"_value","type":"uint256"}],"name":"set","outputs":[{"name":"","type":"bool"}],"payable":false,"type":"function"},{"constant":false,"inputs":[],"name":"get","outputs":[{"name":"storeValue","type":"uint256"}],"payable":false,"type":"function"},{"anonymous":false,"inputs":[{"indexed":false,"name":"_newValue","type":"uint256"},{"indexed":false,"name":"_sender","type":"address"}],"name":"SetComplete","type":"event"}];


const setInputBytecode = abi.encodeMethod(SimpleStoreABI[0], [24000]);

// returns 0x60fe47b10000000000000000000000000000000000000000000000000000000000005dc0

const setOutputBytecode = abi.decodeMethod(SimpleStoreABI[0], "0x0000000000000000000000000000000000000000000000000000000000000001");

// returns  Result { '0': true }



const getInputBytecode = abi.encodeMethod(SimpleStoreABI[1], []);

// returns 0x6d4ce63c

const getMethodOutputBytecode = abi.decodeMethod(SimpleStoreABI[1], "0x000000000000000000000000000000000000000000000000000000000000b26e");

// returns Result { '0': <BN: b26e>, storeValue: <BN: b26e> }

const SetCompleteInputBytecode = abi.encodeEvent(SimpleStoreABI[2], [24000, "0xca35b7d915458ef540ade6068dfe2f44e8fa733c"]);

// returns 0x10e8e9bc0000000000000000000000000000000000000000000000000000000000005dc0000000000000000000000000ca35b7d915458ef540ade6068dfe2f44e8fa733c

const event = abi.decodeEvent(SimpleStoreABI[2], "0x0000000000000000000000000000000000000000000000000000000000000d7d000000000000000000000000ca35b7d915458ef540ade6068dfe2f44e8fa733c");

/* returns   Result {
  '0': <BN: d7d>,
  '1': '0xca35b7d915458ef540ade6068dfe2f44e8fa733c',
  _newValue: <BN: d7d>,
  _sender: '0xca35b7d915458ef540ade6068dfe2f44e8fa733c' }
*/

const event = abi.decodeLogItem(SimpleStoreABI[2], receipt.log[0]);

/* returns   Result {
  '0': <BN: d7d>,
  '1': '0xca35b7d915458ef540ade6068dfe2f44e8fa733c',
  _newValue: <BN: d7d>,
  _sender: '0xca35b7d915458ef540ade6068dfe2f44e8fa733c' }
*/

const decoder = abi.logDecoder(SimpeStoreABI)

const events = decoder(receipt.logs)
/* returns   Result [{
  '0': <BN: d7d>,
  '1': '0xca35b7d915458ef540ade6068dfe2f44e8fa733c',
  _newValue: <BN: d7d>,
  _sender: '0xca35b7d915458ef540ade6068dfe2f44e8fa733c' 
}]
*/


```

## BN

This module returns `BN`s for all quantity values. However, the module supports both BN.js and BigNumber.js modules for encoding, but will only return `BN` instances while decoding.

## Safe Object Returns (decoding)

Instead of returning an array of params as decoded from the method, `fourtwozerojs-abi` will return a safe object that allows you to both get a value at it's return index or optionally, its name.

For example:

```js
{
  '0': <BN: d7d>,
  '1': '0xca35b7d915458ef540ade6068dfe2f44e8fa733c',
  _newValue: <BN: d7d>,
  _sender: '0xca35b7d915458ef540ade6068dfe2f44e8fa733c' }
```

## Why BN.js?

`fourtwozerojs` has made a policy of using `BN.js` across all of its repositories. Here are some of the reasons why:

  1. lighter than alternatives (BigNumber.js)
  2. faster than most alternatives, see [benchmarks](https://github.com/indutny/bn.js/issues/89)
  3. used by the 420coin foundation across all [`fourtwentyjs`](https://github.com/420integrated/fourtwentyjs) repositories
  4. is already used by a critical JS dependency of many 420coin packages, see package `elliptic`
  5. purposefully **does not support decimals or floats numbers** (for greater precision), remember, the 420coin blockchain cannot and will not support float values or decimal numbers.

## Browser Builds

`fourtwozerojs` provides production distributions for all of its modules that are ready for use in the browser right away. Simply include either `dist/fourtwozerojs-abi.js` or `dist/fourtwozerojs-abi.min.js` directly into an HTML file to start using this module. Note, an `fourtwentyAbi` object is made available globally.

```html
<script type="text/javascript" src="fourtwozerojs-abi.min.js"></script>
<script type="text/javascript">
new fourtwentyAbi(...);
</script>
```

Note, even though `fourtwozerojs` should have transformed and polyfilled most of the requirements to run this module across most modern browsers. You may want to look at an additional polyfill for extra support.

Use a polyfill service such as `Polyfill.io` to ensure complete cross-browser support:
https://polyfill.io/


