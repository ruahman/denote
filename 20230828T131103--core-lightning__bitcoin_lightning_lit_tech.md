---
title:      "core lightning"
date:       2023-08-28T13:11:03-04:00
tags:       ["bitcoin", "lightning", "lit", "tech"]
identifier: "20230828T131103"
---


start running lightning

``` shell
bitcoind &
./lightningd/lightningd &
./cli/lightning-cli help
```

run lightning on testnet

``` shell
$ bitcoind -testnet &
$ lightningd --network=testnet
```

### verify core lightning ###

check to see if yo already have lightning running
`ps auxww | grep -i lightning`

core lightning does not like pruned nodes.

verify if you installed lightningd correctly
`lightningd --help`

list all configuration options
`lightning-cli listconfigs`

show all available funds from internal wallet
`lightning-cli listfunds`

list transactions we have stored in wallet
`lightning-cli listtransactions`

list/find invoices
`lightning-cli listinvoices`
- find invoices that match {lable}, {payment_hash}, etc, 
- or all if no query parameter is specified

list all node in lightning network found via gossip messages
`lightning-cli listnodes`
