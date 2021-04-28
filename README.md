# Electrum Mnemonics (electrum v2 and greater)

[![Build Status](https://travis-ci.org/bitcoinjs/electrum-mnemonic.png?branch=master)](https://travis-ci.org/bitcoinjs/electrum-mnemonic)
[![NPM](https://img.shields.io/npm/v/electrum-mnemonic.svg)](https://www.npmjs.org/package/electrum-mnemonic)

[![code style: prettier](https://img.shields.io/badge/code_style-prettier-ff69b4.svg?style=flat-square)](https://github.com/prettier/prettier)

Updated for usage with Innova (INN) (Standard (Legacy) Addresses start with a i)

## Install

```bash
npm install electrum-mnemonic
```

## Usage

* Generate a mnemonic (English only for now)

```js
const mn = require('electrum-mnemonic')
console.log(mn.generateMnemonic()) // default segwit bech32 wallet
console.log(mn.generateMnemonic({ prefix: mn.PREFIXES.segwit })) // explicit segwit bech32 wallet
console.log(mn.generateMnemonic({ prefix: mn.PREFIXES.standard })) // legacy p2pkh wallet (base58 address starting with 1)
console.log(mn.generateMnemonic({ prefix: mn.PREFIXES['2fa'] })) // 2fa legacy
console.log(mn.generateMnemonic({ prefix: mn.PREFIXES['2fa-segwit'] })) // legacy p2pkh wallet (base58 address starting with 1)
```

* Validate mnemonic phrase (default prefix: segwit)

```js
const mn = require('electrum-mnemonic')
const segwitPhrase = mn.generateMnemonic();
const standardPhrase = mn.generateMnemonic({ prefix: mn.PREFIXES.standard });
console.log(mn.validateMnemonic(segwitPhrase, mn.PREFIXES.segwit)); // true
console.log(mn.validateMnemonic(standardPhrase, mn.PREFIXES.standard)); // true
```

* Convert mnemonic into 64 byte seed (for bip32)

```js
const mn = require('electrum-mnemonic')
const phrase = mn.generateMnemonic()
const seed = mn.mnemonicToSeedSync(phrase, { passphrase: 'extrapassword' })
console.log(seed)

// async
;(async () => {
  const seed2 = await mn.mnemonicToSeed(phrase)
  // no extra password so seed and seed2 should be different
  console.log(seed2)
})()
```

* mnemonicToSeed prefix must match the prefix used to generate (default: segwit)

```js
const mn = require('electrum-mnemonic')
const phrase = mn.generateMnemonic({ prefix: mn.PREFIXES.standard })
const seed = mn.mnemonicToSeedSync(phrase, { prefix: mn.PREFIXES.standard })
console.log(seed)
const segwitPhrase = mn.generateMnemonic()
const segwitSeed = mn.mnemonicToSeedSync(phrase)
console.log(segwitSeed)
```

* Skip valid version prefix check

```js
const mn = require('electrum-mnemonic')
const seed = mn.mnemonicToSeedSync('abc', { skipCheck: true })
console.log(seed)
```

* Custom prefixes for checking

```js
const mn = require('electrum-mnemonic')
const phrase = mn.generateMnemonic({ prefix: 'fed' })
const seed = mn.mnemonicToSeedSync(phrase, { prefix: 'fed' })
console.log(seed)
```
