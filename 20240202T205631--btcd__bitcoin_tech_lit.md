---
title:      "btcd"
date:       2024-02-02T20:56:31-04:00
tags:       ["bitcoin", "tech", "lit"]
identifier: "20240202T205631"
---

Btcd implements most of the core functions of a node except wallet and chain functionality.

Btcd  unbundled the wallet functionality into a separate wallet application called btcwallet. 

btcd client also has minor differences from bitcoind such as enabling TLS for RPC connections by default 
and accepting both HTTP requests and Websockets. 

Decided to separate chain and wallet architecture

split the chain from the wallet.

One of the major problems with wallet and chain functionality integrated in the same process is multi-user support. 

For example, when using bitcoind or bitcoin-qt, two users sharing the same computer must each maintain their own block chain. 

btcd is designed to provide chain services for separate wallet processes.

