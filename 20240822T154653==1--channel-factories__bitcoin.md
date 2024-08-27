---
title:      #("channel-factories" 0 17 (ivy-index 0))
date:       2024-08-22T15:46:53-04:00
tags:       ["bitcoin"]
identifier: "20240822T154653"
---

What is it?

are a mult-user contract capable of opening payment channels without
putting the channel-open transaction onchain.


hook, allocation, commitment 

multi-party channel

two-party payment channels

The subchannels are payment channels

with Channel factories, 
we move the funding and closing transaction for the payment channels
off-chain too.

in this context, we call the funding transactions allocation transactions.

each allocation transaction funds a number of two-party payment channels

The body of the channel factory is a mult-party channel

The channel factory can rebalance and splice channels by replacing the allocation transaction with a new one.

the funding transaction for the channel factory itself is called a 
hook transaction

we use taproot everywhere

Policy Language defines each step.
it compiles to miniscript

the protocol for communicating between the participants to create
and sign transactions relies on composable policies and PSBTs.

All channels are duplex

we use taproot outputs, with each script path representing a 
seperate timelock.

the funding outputs in each allocation transaction also use taproot

in the payment channels, all participants are arranged in a hub and 
spoke pattern.

LP(Limited Partner)???

At the end of a month, there is one transaction that closes the channel factory

In the happy path, only two transactions go on-chain, 
the hook transaction and the closing transaction

in the unhappy path, the last commit transactions are resolved on-chain
with timelocks in the final days of the month

MuSig2 and FROST

eltoo  
