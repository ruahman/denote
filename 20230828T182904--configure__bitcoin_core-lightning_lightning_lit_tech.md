---
title:      "configure"
date:       2023-08-28T18:29:04-04:00
tags:       ["bitcoin", "core-lightning", "lightning", "lit", "tech"]
identifier: "20230828T182904"
---

lightningd can be configured via commandline or config file

to use a config file
setup `~./lightning/config`

`network=NETWORK` 
- bitcoin,testnet,signet,regtest

`bitcoin-cli=PATH`
- path to bitcoin-cli

`bitcoin-datadir=DIR`
- -datadir argument for bitcoin-cli

`bitcoin-rpcuser=USER`
- RPC username for talking to bitcoind

`bitcoin-rpcpassword=PASSWORD`
- RPC password for talking with bitcoind

`bitcoin-rpcconnect=HOST`
- RPC host for bitcoind

`bitcoin-rpcport=PORT`
- port to bitcoind host

`bitcoin-retry-timeout=SECONDS`
- number of seconds to wait before retry to connect to bitcoind

## lightning daemon options ##

`lightning-dir=DIR`
- set the working directory

`pid-file=PATH`
- pid file 

`log-level=LEVEL[:SUBSYSTEM]`

`log-prefix=PREFIX`
- prefix for all log lines

`log-file=FILE`
- log to this file

`rpc-file=PATH`
- set JSON-RPC socket for lightning-cli

`rpc-file-mode=MODE`
- set file permision for socket file

`daemon`
- run in background, suppress stdout and stderr, need to specify log-file

`conf=PATH`
- path to config file

`wallet=DNS`
- identify location of the wallet. 
  * sqlite or postgres
    + --wallet=sqlite3://$HOME/.lightning/bitcoin/lightningd.sqlite31
    + --wallet=postgres://user:pass@localhost:5432/db_name
	
`grpc-port=portnum`
- port number of grpc plugin

### lightning node options ###

`alias=NAME`
- 32 bytes of UTF-8 characters

`rgb=RRGGBB`
- color

`fee-base=MILLISATOSHI`
- default is 1000 msats. the base fee to charge for every payment passing through.  

`fee-per-satoshi=MILLIONTHS`
default is 10 = 0.001 %, 1000 = 0.1%  10_000 = 1%,  proportional to amount. 

`min-capacity-sats=SATOSHIS`
- default is 10_000.  minimum amount for a channel

`ignore-fee-limits=BOOL`
- allow node which established channels with us to set any fee they want

`commit-time=MILLISECONDS`
- how long to wait till sending a commitment message to peer

`htlc-minimum-msat=MILLISATOSHIS`
- minimum allowed HTLC value for newly created channels

`htlc-maximum-msat=MILISATOSHIS`
- maximum allow=ed HTLC for a newly created channel

`disable-ip-discovery`
- turn off public IP discovery to send node_announcement

### Lightning Channel and HTLC options ###

`large-channels`
- removes capacity limits for channel creation.

`watchtime-blocks=BLOCKS`
- how long they have to wait before a user can make a unilateral close

`max-locktime-blocks=BLOCKS`
- the longest our funds can be delayed

`funding-confirms=BLOCKS`
- confirmations required for the funding transaction

`commit-fee=PERCENTAGE`
- percentage of commitment transaction

`max-concurrent-htlcs=INTEGER`
- number of HTLCs that one channel can handle

`cltv-delta=BLOCKS`
- number of blocks between incomming payments and outgoing payments

`cltv-final=BLOCKS` 
