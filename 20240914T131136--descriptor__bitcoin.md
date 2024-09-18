---
title:      "descriptor"
date:       2024-09-14T13:11:36-04:00
tags:       ["bitcoin"]
identifier: "20240914T131136"
---

you are likely to be moving addresses between wallets and even setting up wallets to watch over funds controlled by different wallets.

That's where descriptors come in. 

Moving addresses between wallets used to focus on xpub and xprv,

*xpriv*: An extended private key. This is the combination of a private key and a chain code. It's a private key that a whole sequence of children private keys can be derived from.

*xpub*: An extended public key. This is the combination of a public key and a chain code. It's a public key that a whole sequence of children public keys can be derived from.

these are hierarchical keys that can be used to create whole families of keys, built on the idea of HD Wallets.

*HD Wallet* :: This is a hierarchical design where a single seed can be used to generate a whole sequence of keys. The entire wallet may then be restored from that seed, rather than requiring the restoring of every single private key.

*Derivation Path* :: you need to be able to define individual keys as descendents of a seed. For example [0] is the 0th key, [0/1] is the first son of the 0th key, [1/0/1] is the first grandson of the zeroth son of the 1st key

- Some keys also contain a ' after the number, to show they're hardened, which protects them from a specific attack that can be used to derive an xprv from an xpub

a derivation path defines a key, which means that a key represents a derivation path.

xpubs and xprvs proved insufficient when the types of public keys multiplied under the SegWit expansion, thus the need for "output descriptors".

 What is an output descriptor? A precise description of how to derive a Bitcoin address 
 
 Descriptors are visible in several commands such as listunspent and getaddressinfo:
 
```
$ bitcoin-cli getaddressinfo ms7ruzvL4atCu77n47dStMb3of6iScS8kZ
{
  "address": "ms7ruzvL4atCu77n47dStMb3of6iScS8kZ",
  "scriptPubKey": "76a9147f437379bcc66c40745edc1891ea6b3830e1975d88ac",
  "ismine": true,
  "solvable": true,
  "desc": "pkh([d6043800/0'/0'/18']03efdee34c0009fd175f3b20b5e5a5517fd5d16746f2e635b44617adafeaebc388)#4ahsl9pk",
  "iswatchonly": false,
  "isscript": false,
  "iswitness": false,
  "pubkey": "03efdee34c0009fd175f3b20b5e5a5517fd5d16746f2e635b44617adafeaebc388",
  "iscompressed": true,
  "ischange": false,
  "timestamp": 1592335136,
  "hdkeypath": "m/0'/0'/18'",
  "hdseedid": "fdea8e2630f00d29a9d6ff2af7bf5b358d061078",
  "hdmasterfingerprint": "d6043800",
  "labels": [
    ""
  ]
}
```

Here the descriptor is pkh([d6043800/0'/0'/18']03efdee34c0009fd175f3b20b5e5a5517fd5d16746f2e635b44617adafeaebc388)#4ahsl9pk.

A descriptor is broken into several parts:
`function([derivation-path]key)#checksum`
- function ::  The function that is used to create an address from that key. 
  * pkh, sh,  wpkh, wsh
- derivation path :: This describes what part of an HD wallet is being exported.
  * in this case it's a seed with the fingerprint d6043800 and then the 18th child of the 0th child of the 0th child (0'/0'/18') of that seed. 
  * It's worth noting here that if you ever get a derivation path without a fingerprint, you can make it up. It's just that if there's an existing one, you should match it, because if you ever go back to the device that created the fingerprint, you'll need to have the same one.
  
- key ::  The key or keys that are being transferred. 
  * This could be something traditional like an xpub or xprv
  
- checksum :: Descriptors are meant to be human transferrable. This checksum makes sure you got it right.

how you can derive an address from a descriptor
```
$ bitcoin-cli deriveaddresses "pkh([d6043800/0'/0'/18']03efdee34c0009fd175f3b20b5e5a5517fd5d16746f2e635b44617adafeaebc388)#4ahsl9pk"
[
  "ms7ruzvL4atCu77n47dStMb3of6iScS8kZ"
]
```
- *deriveaddresses* takes as input a descriptor and computes the corresponding addresses.

Output descriptors currently support:

- pk :: (P2PK) :: Pay to public key
- pkh :: (P2PKH) :: Pay to public key 
- wpkh :: (P2WPKH) :: Pay to witness public key hash
- sh :: (P2SH) :: Pay to script hash
- wsh :: (P2WSH) :: Pay to witness script hash
- tr :: (P2TR) :: Pay to taproot
- multi :: (Multisig) 
- sortedmulti :: Multisig scripts where the public keys are sorted lexicographically
- multi_a, sortedmulti_a :: Multisig for inside taproot script tree

# examples 
`pk(0279be667ef9dcbbac55a06295ce870b07029bfcdb2dce28d959f2815b16f81798)`
- describes a P2PK output with specified public key

`pkh(02c6047f9441ed7d6d3045406e95c07cd85c778e4b8cef3ca7abac09b95c709ee5)`
-  describes a P2PKH output with the specified public key.

`wpkh(02f9308a019258c31049344f85f89d5229b531c845836f99b08601f113bce036f9)`
- describes a P2WPKH output with the specified public key.

`sh(wpkh(03fff97bd5755eeea420453a14355235d382f6472f8568a18b2f057a1460297556))`
-  describes a P2SH-P2WPKH output with the specified public key.

`multi(1,022f8bde4d1a07209355b4a7250a5c5128e88b84bddc619ab7cba8d569b240efe4,025cbdf0646e5db4eaa398f365f2ea7a0e3d419b7e0330e39ce92bddedcac4f9bc)` 
- describes a bare 1-of-2 multisig output with keys in the specified order.

`sh(multi(2,022f01e5e15cca351daff3843fb70f3c2f0a1bdd05e5af888a67784ef3e10a2a01,03acd484e2f0c7f65309ad178a9f559abde09796974c57e714c35f110dfc27ccbe))`
- describes a P2SH 2-of-2 multisig output with keys in the specified order.

`sh(sortedmulti(2,03acd484e2f0c7f65309ad178a9f559abde09796974c57e714c35f110dfc27ccbe,022f01e5e15cca351daff3843fb70f3c2f0a1bdd05e5af888a67784ef3e10a2a01))`
-  describes a P2SH 2-of-2 multisig output with keys sorted lexicographically in the resulting redeemScript.

`wsh(multi(2,03a0434d9e47f3c86235477c7b1ae6ae5d3442d49b1943c2b752a68e2a47e247c7,03774ae7f858a9411e5ef4246b70c65aac5649980be5c17891bbec17895da008cb,03d01115d548e7561b15c38f004d734633687cf4419620095bc5b0f47070afe85a))`
- describes a P2WSH 2-of-3 multisig output with keys in the specified order.

`pk(xpub661MyMwAqRbcFtXgS5sYJABqqG9YLmC4Q1Rdap9gSE8NqtwybGhePY2gZ29ESFjqJoCu1Rupje8YtGqsefD265TMg7usUDFdp6W1EGMcet8)` 
- describes a P2PK output with the public key of the specified xpub.

`pkh(xpub68Gmy5EdvgibQVfPdqkBBCHxA5htiqg55crXYuXoQRKfDBFA1WEjWgP6LHhwBZeNK1VTsfTFUHCdrfp1bgwQ9xv5ski8PX9rL2dZXvgGDnw/1/2)` 
- describes a P2PKH output with child key 1/2 of the specified xpub.

`pkh([d34db33f/44'/0'/0']xpub6ERApfZwUNrhLCkDtcHTcxd75RbzS1ed54G1LkBUHQVHQKqhMkhgbmJbZRkrgZw4koxb5JaHWkY4ALHY2grBGRjaDMzQLcgJvLJuZZvRcEL/1/*)` 
- describes a set of P2PKH outputs, but additionally specifies that the specified xpub is a child of a master with fingerprint d34db33f, and derived using path 44'/0'/0'.

`wsh(multi(1,xpub661MyMwAqRbcFW31YEwpkMuc5THy2PSt5bDMsktWQcFF8syAmRUapSCGu8ED9W6oDMSgv6Zz8idoc4a6mr8BDzTJY47LJhkJ8UB7WEGuduB/1/0/*,xpub69H7F5d8KSRgmmdJg2KhpAK8SR3DjMwAdkxj3ZuxV27CprR9LgpeyGmXUbC6wb7ERfvrnKZjXoUmmDznezpbZb7ap6r1D3tgFxHmwMkQTPH/0/0/*))` 
- describes a set of 1-of-2 P2WSH multisig outputs where the first multisig key is the 1/0/i child of the first specified xpub and the second multisig key is the 0/0/i child of the second specified xpub, and i is any number in a configurable range (0-1000 by default).

`wsh(sortedmulti(1,xpub661MyMwAqRbcFW31YEwpkMuc5THy2PSt5bDMsktWQcFF8syAmRUapSCGu8ED9W6oDMSgv6Zz8idoc4a6mr8BDzTJY47LJhkJ8UB7WEGuduB/1/0/*,xpub69H7F5d8KSRgmmdJg2KhpAK8SR3DjMwAdkxj3ZuxV27CprR9LgpeyGmXUbC6wb7ERfvrnKZjXoUmmDznezpbZb7ap6r1D3tgFxHmwMkQTPH/0/0/*))` 
- describes a set of 1-of-2 P2WSH multisig outputs where one multisig key is the 1/0/i child of the first specified xpub and the other multisig key is the 0/0/i child of the second specified xpub, and i is any number in a configurable range (0-1000 by default).

- The order of public keys in the resulting witnessScripts is determined by the lexicographic order of the public keys at that index.

`tr(c6047f9441ed7d6d3045406e95c07cd85c778e4b8cef3ca7abac09b95c709ee5,{pk(fff97bd5755eeea420453a14355235d382f6472f8568a18b2f057a1460297556),pk(e493dbf1c10d80f3581e4904930b1404cc6c13900ee0758474fa94abe8c4cd13)})` 
-  describes a P2TR output with the c6... x-only pubkey as internal key, and two script paths.

`tr(c6047f9441ed7d6d3045406e95c07cd85c778e4b8cef3ca7abac09b95c709ee5,sortedmulti_a(2,2f8bde4d1a07209355b4a7250a5c5128e88b84bddc619ab7cba8d569b240efe4,5cbdf0646e5db4eaa398f365f2ea7a0e3d419b7e0330e39ce92bddedcac4f9bc)) ` 
-  describes a P2TR output with the c6... x-only pubkey as internal key, and a single multi_a script that needs 2 signatures with 2 specified x-only keys, which will be sorted lexicographically.

`wsh(sortedmulti(2,[6f53d49c/44h/1h/0h]tpubDDjsCRDQ9YzyaAq9rspCfq8RZFrWoBpYnLxK6sS2hS2yukqSczgcYiur8Scx4Hd5AZatxTuzMtJQJhchufv1FRFanLqUP7JHwusSSpfcEp2/0/*,[e6807791/44h/1h/0h]tpubDDAfvogaaAxaFJ6c15ht7Tq6ZmiqFYfrSmZsHu7tHXBgnjMZSHAeHSwhvjARNA6Qybon4ksPksjRbPDVp7yXA1KjTjSd5x18KHqbppnXP1s/0/*,[367c9cfa/44h/1h/0h]tpubDDtPnSgWYk8dDnaDwnof4ehcnjuL5VoUt1eW2MoAed1grPHuXPDnkX1fWMvXfcz3NqFxPbhqNZ3QBdYjLz2hABeM9Z2oqMR1Gt2HHYDoCgh/0/*))#av0kxgw0` 
- describes a 2-of-3 multisig.

# Reference

* `sh(SCRIPT)` :: P2SH
* `wsh(SCRIPT)` :: P2WSH
* `pk(KEY)` :: P2PK
* `pkh(KEY)` :: P2PKH
  - use `addr` if you only know the pubkey hash
* `wpkh(KEY)` :: P2WPKH
* `multi(k,KEY_1,KEY_2,....,KEY_n)` :: k-of-n multisig
  - not inside tr
* `sortedmulti(k,KEY_1,KEY_2,...,KEY_n)` :: k-of-n multisig with keys
  sorted lexicographically
  - not inside tr
* `multi_a(k,KEY_1,KEY_2,...,KEY_n)` :: k-of-n multisig script 
  - only inside tr
* `sortedmulti_a(k,KEY_1,KEY_2,...,KEY_N)` :: k-of-n keys sorted
  - only inside tr
* `tr(KEY)` or `tr(KEY,TREE)` :: P2TR output with the specified key
   as internal key, and optionally a tree of script paths.
   
Whenever a public key is described using a hardened derivation step, the script cannot be computed without access to the corresponding private key.

, this descriptor does not let you compute scripts without access to the corresponding private keys

Every public key can be prefixed by an 8-character hexadecimal fingerprint plus optional derivation steps 

