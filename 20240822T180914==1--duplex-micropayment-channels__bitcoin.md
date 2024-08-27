---
title:      "duplex-micropayment-channels"
date:       2024-08-22T18:09:14-04:00
tags:       ["bitcoin"]
identifier: "20240822T180914"
---

how do we make sure that the latest agreed state is what is broadcasted to the blockchain

duplex micropayment channel
- they have a limited time life

invalidation tree????

timelock
- ex: transaction will only be valid after day 100

each susequent transaction has a timelock slightly smaller than the previous
setilment transaction

make sure that you submit the latest transaction before any of the privious
settlement transactions timelock expires

collaborative close 
- without a timelock

unilateral close
- has a timelock

Lightning Penalty
- instead of creating on trnasaction, two are created
- revocation secrets
- there is a problem with backups

eltoo

SIGHASH_NOINPUT
