---
title:      "HTLC"
date:       2024-09-09T16:36:35-04:00
tags:       ["bitcoin", "lightning"]
identifier: "20240909T163635"
---

This post will give an overview of HTLCs and how they allow multi-hop payments to be made

Alice can use her channel with Bob to route a payment to Charlie since Bob has a channel with Charlie.

# Step 1: Generating and sharing the pre-image hash

Alice first needs to tell Charlie that she wants to pay him. 

Charlie will then generate a random secret, **S**, and get the hash of S which we will call **H**. Charlie then sends **H** to Alice.


Alice is _**the HTLC offerer**_ and Bob is the _**HTLC receiver**_.

 Alice will construct her commitment transaction to now include the HTLC. The commitment transaction will have three outputs:
 
1. One 5 BTC output to Bob (spendable immediately).

2. One 3 BTC output with two possible spending paths: One spendable by Alice after a _**to_self_delay**_ and one immediately spendable by Bob if he has the required _**revocation key**_ 

3. One 2 BTC output, This output is where the HTLC magic must happen. 




# HTLC deep dive  

 The commitment transactions that each participant holds looks slightly different to that of their peer in that any output going to the local node must be encumbered by a relative time lock of **_to_self_delay_**. This is to give the other party a chance to spend along the revocation path of the output if they need.

 let’s look at how HTLC’s will fit into all of this.

 Alice is sending 2 BTC to a recipient and is using her channel with Bob as the first hop in the route

Remember that Alice has been given a hash, _**H**_, to which she needs to pay. In this example, Alice is _**the HTLC offerer**_ and Bob is _**the HTLC receiver**_.

## Zooming in on Alice: ## 

Let’s zoom in on how Alice will construct her commitment transaction to now include the HTLC.

say we start of with 5 BTC on each side

* One 5 BTC output to Bob (spendable immediately).
* One 3 BTC output with two possible spending paths
  - One spendable by Alice after a **_to_self_delay_**
  - One immediately spendable by Bob if he has the required **_revocation_** key
- One 2 BTC output
    + this is where the _**HTLC**_ magic happens
   
This output is where the HTLC magic must happen. We need the following spending paths on this output:

1. It needs to be spendable by Bob if he has the pre-image of **H** (hash-locked path)
2. Or spendable by Alice after **_cltv_expiry_**
3. Or spendable by Bob immediately if he has **_the revocation key_**

BUT remember that Alice’s outputs to herself must always have a relative timelock of **_to_self_delay_** even after **_cltv_expiry_**. Knowing this, let’s update the HTLC spending paths a bit:

Bob could have an extra **_to_self_delay_** blocks in order to sweep the hash-locked output even though the HTLC is technically expired.


 instead of sending funds to Alice directly, the funds are instead sent to a separate HTLC-timeout transaction (signed by both Alice and Bob) and this separate time out transaction then enforces the to_self_delay. 

 

This allows Alice to definitively lock in the fact that the HTLC has expired and removes Bob’s ability to claim the hash-locked output all while still ensuring that Alice can only get her funds after to_self_delay

The final state of the commitment transaction’s HTLC output spending paths is as follows:

* One spending path to Bob if he pre-image of H (hash-locked path)

* One spending path to Bob if he has the revocation key.

* One spending path to a HTLC-timeout transaction

The HTLC-timeout transaction has the following construction:

* one to Alice after to_self_delay
* one to Bob if he can provide the revocation key.

The Script for Alices (the HTLC offerer) commitment transaction’s HTLC output looks as follows:

```
# To remote node with revocation key
OP_DUP OP_HASH160 <RIPEMD160(SHA256(revocationpubkey))> OP_EQUAL
OP_IF
    OP_CHECKSIG
OP_ELSE
    <remote_htlcpubkey> OP_SWAP OP_SIZE 32 OP_EQUAL
    OP_NOTIF
        # To local node via HTLC-timeout transaction (timelocked).
        OP_DROP 2 OP_SWAP <local_htlcpubkey> 2 OP_CHECKMULTISIG
    OP_ELSE
        # To remote node with preimage.
        OP_HASH160 <RIPEMD160(payment_hash)> OP_EQUALVERIFY
        OP_CHECKSIG
    OP_ENDIF
OP_ENDIF
```

You can see in the script above that the first path is the revocation path, the second is the path to the HTLC-timeout transaction (the time-locked path) and the third is the hash-locked spending path.

The HTLC-timeout transaction output script looks as follows:

```
OP_IF
    # Penalty transaction
    <revocationpubkey>
OP_ELSE
    `to_self_delay`
    OP_CHECKSEQUENCEVERIFY
    OP_DROP
    <local_delayedpubkey>
OP_ENDIF
OP_CHECKSIG
```

# Zooming in on Bob

Let’s now zoom in on how Bob (the HTLC receiver) will construct his commitment transaction to include the HTLC.

The commitment transaction will have three outputs:

* One 3 BTC output to Alice (spendable immediately).
* One 5 BTC output with two possible spending paths: One spendable by Bob after a **_to_self_delay_** and one immediately spendable by Alice if she has the required **_revocation key_**
* Again the 2 BTC output on Bob’s commitment transaction is a bit complicated.
* 
a separate HTLC-success transaction is used for this 
allowing Bob to spend from the hash-locked path to this HTLC-success transaction which will then separately enforce the to_self_delay condition.

The final state of the commitment transaction’s HTLC output spending paths is as follows:

* One spending path to Alice if she has the revocation key (revocation path)
* One spending path to Alice after cltv_expiry (time-locked path)
* One spending path to the HTLC-success transaction IF Bob can reveal the pre-image of H (hash-locked path).

The HTLC-timeout transaction has the following construction:

* The transaction is not time locked (unlike in Alice’s case).

* The transaction has one output with two possible spending paths:


* one to Bob after to_self_delay

* one to Alice if she can provide the revocation key.


The Script for Bob’s (the HTLC receiver) commitment transaction’s HTLC output looks as follows:

```
# To remote node with revocation key
OP_DUP OP_HASH160 <RIPEMD160(SHA256(revocationpubkey))> OP_EQUAL
OP_IF
    OP_CHECKSIG
OP_ELSE
    <remote_htlcpubkey> OP_SWAP OP_SIZE 32 OP_EQUAL
    OP_IF
        # To local node via HTLC-success transaction.
        OP_HASH160 <RIPEMD160(payment_hash)> OP_EQUALVERIFY
        2 OP_SWAP <local_htlcpubkey> 2 OP_CHECKMULTISIG
    OP_ELSE
        # To remote node after timeout.
        OP_DROP <cltv_expiry> OP_CHECKLOCKTIMEVERIFY OP_DROP
        OP_CHECKSIG
    OP_ENDIF
OP_ENDIF
```

The HTLC-timeout transaction output script looks as follows:

```
OP_IF
    # Penalty transaction
    <revocationpubkey>
OP_ELSE
    `to_self_delay`
    OP_CHECKSEQUENCEVERIFY
    OP_DROP
    <local_delayedpubkey>
OP_ENDIF
OP_CHECKSIG
```

![image](https://ellemouton.com/lnThings/day7_5.png#center)


# A Technical Walkthrough of Hash Time Locked Contracts and Lightning Channel Operations


we will walk through the different operations of a Lightning channel 

1. First, we explore how Hash Time Locked Contracts (HTLCs) are added to a channel and how channel peers commit to a new state including these HTLCs. 

2. Next, we discuss how a channel’s normal flow is re-established after a disconnection.

3. And finally, we finish with how a cooperative channel closure happens.

# Adding an HTLC

When either Alice or Bob want to send a payment across the channel






























