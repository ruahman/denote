---
title:      "creating-a-channel"
date:       2024-09-09T15:40:15-04:00
tags:       ["bitcoin", "lightning"]
identifier: "20240909T154015"
---

# What is a channel?

It is literally just a transaction sending funds to a 2-of-2 multisig transaction.

This creates an unspent UTXO and the channel is open until 
that UTXO is spent 

During the lifetime of the channel, a bunch of transactions are created that double spend the funding tx and eventually one of those  will go on-chain and the channel will be closed. 

Ideally you only see these two on-chain transactions: one to open the channel and one to close it.

# create a channel

To create a channel we need to somehow get this initial funding channel on-chain. 
- BOLT2 explains how this is done

once a funding transaction is established and confirmed, commitment transactions are used to define the state that the channel is in (how the funds are distributed between the participants in the channel).

. So each commitment transaction pretty much just spends the funding transaction (uses the funding tx as its input) and then has outputs that define the division of funds between the participants.

The funding transaction for a channel between Alice and Bob is simply a transaction that has an output of the following form:

`2 <pubkeyA> <pubkeyB> 2 OP_CHECKMULTISIG`

## `open_channel`
Alice sends this message to Bob to indicate that she wants to open a channel with him.

- funding_pubkey. This is the public key that Alice intends to use as her public key in the funding transaction script.

## *accept_channel*

If Bob is happy with the terms that Alice has put forward in her channel offer, then he can send back the *accept_channel* message which also contains some of his requirements along with the funding_pubkey that he intends to use.

At this point, Alice has all that she needs to construct the funding transaction. 
- However, she at this moment still does not broadcast the funding transaction because she still has no guarantee that Bob will not disappear.
-  So what she needs is a commitment transaction signed by Bob that spends from the funding transaction and divides the channel balance accordingly.

What Alice does now is construct the funding transaction (using Segwit inputs only so that the TXID of the transaction can not be changed due to script sig field malleation) but she does not broadcast the transaction

# *funding_created* 

This message contains the TXID of the funding transaction, the relevant output index of the funding transaction along with a signature for Bob’s commitment transaction

- Note that Bob cannot yet do anything with his commitment transaction since it is spending from a transaction that is not on the blockchain yet.

# *funding_signed* 

If Bob is happy then he can send Alice a funding_signed message.
- This message will contain a Bob’s signature for Alice’s commitment transaction.

# channel_ready

Both parties will be monitoring the blockchain at this point waiting for the funding transaction to be confirmed. Once each party sees it, they will send the other party the channel_ready message which contains the channel ID of the channel.


The channel is now open. YEET!


