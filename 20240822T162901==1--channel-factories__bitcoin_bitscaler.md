---
title:      #("channel-factories" 0 17 (ivy-index 0))
date:       2024-08-22T16:29:01-04:00
tags:       ["bitcoin", "bitscaler"]
identifier: "20240822T162901"
---

BitScaler: Scaling Bitcoin for DeFi

a novel framework aimed at enhancing Bitcoin's scalability for 
cross-chain application.

By integrating channel factories, Taproot, and a ploicy language

BitScaler introduces the ability to create multiple peer-to-peer channels
from a single on-chain transaction

Automated Market Makers(AMMs)

MuSig2

FROST

DLCs

and Eltoo

the concept of channel factories, allows for the creation and closure
of multiple channels from a single on-chain transaction.  (L3)

bitcoin fund to the factory in an initial on-chain transaction called
a hook transaction.

from then on, they can create and close channels amongst themselves,
keeping each channel's transactions entirely off-chain,  including
the transactions that fund these channels.

this is called the Allocation transaction.

it takes the Hook transaction as an input

Hook, allocation, commitment  transactions

Each allocation transaction funds a number of channels

duplex micropayment channels
invalidation tree

the hook transaction in BitScalar uses a taproot script for it's UTXO

the internal key is a NUMS.
