---
title:      "open-and-announce-channel"
date:       2024-09-17T14:06:15-04:00
tags:       ["bitcoin", "lightning"]
identifier: "20240917T140615"
---

we will go over the process of opening a Lightning Network channel as it works today (pre taproot channels).

I will start with the **_channel_open_** message

and end with **_the channel_announcement_** message.

Alice and Bob are both Lightning Network nodes. 

Alice’s node ID (ie, the public key used to identify her node) is alice_node_id

Bob’s node ID is bob_node_id.

Their main two goals are:

1. They want to open a channel between themselves in a trustless way.

2. They want to be able to advertise their new channel so that the rest of the network can use it for routing.

# Opening the channel

A channel is opened once both parties have the ability to fully sign their respective commitment transactions and once the funding transaction is on chain.

There are three transactions in play for the opening of a channel. 

The first is the funding transaction which will need to go on chain. 

The other two are the first commitment transactions held by Alice and Bob describing the initial state of the channel.


# The open_channel message 

Alice will now put together an **_open_channel_** message that she will send to Bob. 


 for Bob to indicate to Alice his acceptance of the request by sending the next message: **_accept_channel_**

When Alice gets this message from Bob, she can now complete the funding transaction’s output and can create the signatures for the inputs. Since everything is filled in, the TXID for the funding tx is now also known.

Alice can now also further and fill in her own commitment transaction:

 This is where the next message comes in: **_funding_created_**.

Alice will now use the funding_created message to tell Bob the TXID and index of the funding transaction along with her signature for Bob’s commitment transaction

Once the funding message has been received, Bob can fill in the rest of his commitment transaction:

Alice won’t broadcast the funding transaction until she has a valid signature from Bob for her commitment transaction. 

 Enter funding_signed:

This was the last piece of the puzzle for Alice. She now has all the info she needs to be able to sign her commitment transaction if ever needed.

Alice can now safely broadcast the funding transaction. Both she and Bob will watch the chain for the confirmation of the funding transaction. Once it has reached the minimum_depth specified by Bob in accept_channel, both sides will exchange the channel_ready message


Ok cool! We have completed our first goal: Alice and Bob have opened a channel between themselves in a trust-less way. Now we move on to step 2: announcing this channel to the network!

# Announcing the channel 


This part is fairly painless. Basically there is just one message, channel_announcement, that Alice and Bob need to construct together and once it is complete, then they can broadcast it to the network. 

Other nodes will use this message to prove a few things:

1. That the channel funding tx is actually an existing, unspent UTXO with an acceptable number of confirmations.

2. That the funding transaction output actually looks like a lightning channel funding transaction

3. That the channel is actually owned by the keys that Alice and Bob say they used to construct the channel.

4. That Alice and Bob both agree on the message being broadcast.

Let’s go over the steps that a node (Charlie) receiving this message will go through in order to verify the new channel that Alice and Bob claimed to have opened.

1. First, Charlie will use the short_channel_id included in the message to make sure that the channel’s funding transaction actually exists on-chain, that it has a sufficient number of confirmations and that it is in fact unspent.






