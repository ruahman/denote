---
title:      "update-state"
date:       2024-09-09T16:00:49-04:00
tags:       ["bitcoin", "lightning"]
identifier: "20240909T160049"
---

This post will describe the commitment transactions in more detail and will also show how the participants of the channel can agree on a new division of funds. In other words: how they create new commitment transaction that splits the funds differently and at the same time invalidate the older commitment transactions that they have.

each party in the channel will hold a commitment transaction representing the state of the channel.

The commitment transactions held by each party vary slightly (details to follow) and this makes it clear which party broadcasted their commitment transaction and makes it possible for the correct party to be punished if they broadcast an invalid state. 

to set up their initial commitment transactions, each party will first create temporary private keys (dA1 for Alice and dB1 for Bob) and calculate their associated public key (PA1 and PB1). 

Alice and Bob will then send each other the temporary public keys. At this point, both parties can construct their own commitment transaction.

Alice’s transaction will look as follows:

1. It will spend from the funding transaction.
2. It will have 2 outputs. (or more. HTLC outputs to come!)
3. The to_remote output will send Bob his 5 BTC immediately.
4. the to_local output is fancier: It either sends Alice her 5 BTC after an   OP_CSV to_self_delay or it can immediately be spent by Bob if he is able to prove that he has Alices temporary private key dA1.

Now Alice wants to send Bob 1 BTC using their channel. So just like for stage 1, both parties will now generate new temporary private keys (dA2 for Alice and dB2 for Bob), calculate their associated public key (PA2 and PB2) and share the public keys with each other. And again, both parties will create commitment transactions to reflect this new state where Alice has transferred one of the BTC to Bob. 

To invalidate this old state and to prove to Bob that she is committing to this new state where she has paid him, Alice will send Bob her initial temporary private key (dA1). Since Bob now has this key, if Alice ever posts the old state, Bob will be able to spend Alice’s to_local output before she is able to claim it. 
