---
title:      #("channel-factories" 0 17 (ivy-index 0))
date:       2024-08-22T16:29:01-04:00
tags:       ["bitcoin", "bitscaler"]
identifier: "20240822T162901"
---

# introduction #

BitScaler: Scaling Bitcoin for DeFi

a novel framework aimed at enhancing Bitcoin's scalability for 
cross-chain application.

By integrating channel factories, Taproot, and a ploicy language

BitScaler introduces the ability to create multiple peer-to-peer channels
from a single on-chain transaction

the necessity for BitScaler arises from the challenge of implementing
Automated Market Makers (AMM) on Bitcoin in conjuction with 
the Lightning Network (LN)

hub-and-spoke architecture

this creates a framework that can be leveraged to create a wide range
of applications.  thereby expanding Bitcoin't potential to 
support a diverse array of DeFi applications.

an off-chain permissionless framework for expanding Bitcoin's applications 

BitScaler introduces the ability to create multiple peer-to-peer channels
from a single on-chain transaction.

Automated Market Makers(AMMs)

MuSig2

FROST

DLCs

and Eltoo

# Channel Factory Basics #


the subchannels my be ordinary 2-of-2 or also another multi-party channel.

Both two-party and multi-party channels (MPC) may use timelocks and 
invalidation trees to enable proper channel state transitions.

Invalidation trees allow for a larger number of allocation transactions to
be used in the Multi Party Channel (MPC) than would otherwise be possible
it one were to merely decrement the timelocks.
- there is only one valid path throught the tree.

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

# BitScaler #

BitScaler extends channel factory design and implement several enhancements
like

non-custodial delegation and simplifications to make it suitable for 
applications 


## 3.1 Taproot ##

the hook transaction uses taproot script for its UTXO
- each branch of the taproot script consistes of an n-of-n multisig
  with distint keys and timelock???
  
the internal key is a NUMS.

A new allocation transaction or node in the invalidation tree 
needs to have an input the unlock the taproot script

## 3.2 Hub and Spoke ##

to serve the purpose of building an Automated Market Maker (AMM),
bitscaler uses only two-party sub-channels, each of which
we use for atomic swaps.

A set of Liguidity providers (LPs) for the AMM are participants in 
a BitScaler channel factory.

There is one additional participant controlled by a federation of validators.

We arrang the two-party channels in a hub-and-spoke pattern,
where the hub corresponds to the validator federation and 
the spoke to the LP(liquidity providers)
- making a total of (n+1) participants

this design helps federeation monitor all the liquidity locked in the 
channels, as well as help copose swaps.

our hook transaction is (n+1)-of-(n+1)

## 3.3 Atomic Swaps ##

## 3.4 Delegation ##

## 3.5 Policy language ##

Policy language allows the channel factory to easily compose sub-policies into larger policies used
for the construction of taproot output descriptors

Policies are compiled into miniscript strings
- which then translates into Bitcoin scripts
