---
title:      "running a node"
date:       2023-08-29T18:30:41-04:00
tags:       ["bitcoin", "core-lightning", "lit", "tech"]
identifier: "20230829T183041"
---

run a bitcoind and wait for it to sync

``` shell
bitcoind -daemon
```

## start lightningd ##

``` shell
lightningd --network=bitcoin --log-level=debug
```

## track logs ##

``` shell
tail -f /mnt/hd1/lightning/logs
```

## get info ##

``` shell
lightning-cli getinfo
```

Core Lightning exposes a **JSON-RPC 2.0** interface over a unix socket
- lightning-cli accesses it

- lightning-newaddr :: generate new address which can be used to fund channels
- lightning-listfunds :: show all funds currently managed by core lightning.
- lightning-connect :: command for connecting to another lightning node.
  + establish connection with another node in the lightning network.
- lightning-fundchannel :: opens a payment channel with a peer
- lightning-invoice :: create an invoice to get paid by another node
- lightning-pay :: pay some one else invoice



## open channel ##

1. you need to transfer some funds to lightingd so that it can open a channel.

``` shell
# Returns an address <address>
lightning-cli newaddr
```

confirm that lightningd got funds by

``` shell
# Returns an array of on-chain funds.
lightning-cli listfunds
```

2. once lightningd has funds we can open channels

``` shell
lightning-cli connect <node_id> <ip> [<port>]
lightning-cli fundchannel <node_id> <amount_in_satoshis>
```

connect to a peer

``` shell
lightning-cli connect <peer>@<addr>:<port>
lightning-cli connect 03e347d089c071c27680e26299223e80a740cf3e3fc4b4237fa219bb67121a670b@45.33.22.210:9735
```

3. you can check status of channel with

``` shell
lightning-cli listpeers
```


## sending a receiving payments ##

create an invoice

``` shell
lightning-cli invoice <amount> <label> <description>
```

you can decode the invoice

``` shell
lightning-cli decode <invoice>
```

you can then pay the invoice with

``` shell
lightning-cli pay <bolt11>
```


Fund you c-lightning wallet
===========================

first you have to fund the wallet associated with your lightning node

create an address to fund your wallet
`lightning-cli newaddr`

> {
   "bech32": "tb1qzh6mwq0wx45602avsqk3p42pd8e26c4pgtdd4z"
}

- you can then send money to this address

list funds to your wallet
` lightning-cli listfunds`
- wait after 6 confirmations 


Connect to a remote node
========================

connect to another node
`lightning-cli connect id@host:port`


Open a Channel
--------------

fund a channel
`lightning-cli fundchannel id sats feerate announce minconf`

confirm channel status with
`lightning-cli listfunds`
- once it is fundes it will show *short_channel_id*
  + it of the form "block x txid x vout"


# fund a channel example #
`lightning-cli fundchannel id=03255c5b3b8422ae293c950a843888b8d47a7d04003c73964c2f78d106e89832f1 amount=20000 feerate=minimum announce=true minconf=0 push_msat=1000000`
- setting up a zero-conf channel that pushes an about.


# listpeer #

`lightning-cli listpeers | jq '.peers[] | {id:.id,connected:.connected,num_channels:.num_channels,netaddr:.netaddr,channels:.channels | [.[] | {channel_id:.channel_id, short_channel_id:.short_channel_id, status:.status}]}'`

# listpeerchannels #

`lightning-cli listpeerchannels | jq '.channels[] | {peer_id:.peer_id, peer_connected:.peer_connected, status:.status, channel_id:.channel_id,short_channel_id:.short_channel_id,total_msat:.total_msat,their_reserve_msat:.their_reserve_msat,our_reserve_msat:.our_reserve_msat,spendable_msat:.spendable_msat,receivable_msat:.receivable_msat}'`


# invoice #

`lightning-cli invoice 2000 label="test#2" description="give more money to workit"`


# listinvoices #
- list invoices you created

`lightning-cli listinvoices`

# listpays #
- list payments you made

`lightning-cli listpays`

# close channel #

`lightning-cli close <id>`
