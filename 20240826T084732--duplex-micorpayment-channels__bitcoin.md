---
title:      "duplex-micorpayment-channels"
date:       2024-08-26T08:47:32-04:00
tags:       ["bitcoin"]
identifier: "20240826T084732"
---

duplex micropayment channels

credit card companies can do 10,000 transactions per second

bitcoin and only do 1 transaction per second.

the fundamental data unit tracked by the network is the output,
a tuple consisting of a value denominated in bitcoins and 
an output script.
- the output script sets up a claiming condition that has to be satified
  in order to claim the bitcoins associated with the output.
  
the balance of an address is the sum of all the outputs whose
scripts require the address signature.

the only operation that may modify the global state is a transaction
- a transaction claim one or more previously unclaimed outputs and 
  creates a new output.
- a transaction may redistribute the sum of values to new outputs
  and setup arbitrary claiming conditions for those outputs

off-chain transaction protocols are examples of smart contracts.
- allows business logic to be encoded in the transaction
- the blockchain acts as a conflict mediator

we must have a way to invalidate previous aggreements
- each update creates a new set of transactions that supersede the 
  previous update

# Timelocks and Invalidation

Bitcoin provides a mechanism to make transactions invalid until
some time in the future: _timelocks_ 
- expressed in unix time or block height

Peers in the network discard transactions with future timelocks

a transaction with timelock T can be superseded by another transactions,
spending the same outputs, 

you must broadcast the superseding transaction before the previous transaction
is valid

only the transaction with smallest timelock will eventually be committed.

# 3.3 Shared Accounts

a multisig is ususally is often used to setup a smart contract.

you need two transactions to start.
- a funding transaction and a refund transaction.

# 3.4 Simple Micropayment Channels

# 3.5 Atomic Multiparty Opt-In

colaboratly close???

# Hashed Timelock Contracts (HTLC)

the recipient of a payment must reveal a secret in order to claim
an output before it is refunded to the sender.
- the ability of the recipient to claim the output is dependent 
  on its ability to reveal the secret


# 4 Duplex Micro payment Channel

the fundamental structure of the DMC is the *invalidation tree* 

is a tree where multisig outputs are the nodes of the tree,
connected by transactions as edges.

each transacton is the tree is given a timelock

there is a unique minimal timelock among the sibling transactions

we can invalidate an entire subtree, by adding a new transaction
spending the subtree root ouput, with a smaller timelock than 
all existing transactions.


for example, first setup you funds then build a chain of transaction each
starting with a timelock of 100

each branch invalidates the whole branch that came before.

the number of resets we can do becomes exponential.

for example 11 chained transactions has to potential to make a trilliat update transactions
