---
title:      "channel-factories"
date:       2024-08-22T08:19:04-04:00
tags:       ["bitcoin", "whitepaper"]
identifier: "20240822T081904"
---

Bitcoin has a scalability problem
- does not allow for scaling to a worldwide payment system.

allow rapid changes of the allocation of funds to channels
and reduces the cost of opening new channels.

Instead of one transaction per channel, each user only needs one
transaction to enter a group of nodes
- within the group the user can create arbitrarily as many channels as he wants

If a node crashes or stops cooperating, the latest settlement transaction
can be submitted to the blockchain and enforce the last 
agreed-on state.

Payment channels will not appear in the blockchain, except in the 
case of disputes

Users will be able to enter the system with on blockchain transaction
and then open many channels without further blockchain contact.

atomic transfers / atomic swaps

Payment channels will not appear in the blockchain except in the case of 
disputes

The first commitment transaction is signed before the funding transaction
to ensure that no funds can be taken hostage by on party.

both parties can close the channel at any time by broadcasting the latest 
aggreed upon settelment/commitment transaction.

a commitment/settlement transaction can not be created without both
parties agreeing to it and signing it.

Users will be able to enter the system with one blockchain transaction 
and then create as many channels as he likes without further 
blockchain contact

Funds are committed to a group of users and can be moved between channels
with just a few messages inside the collaborating group

The first commitment is signed before the funding transaction is broadcast
- to ensure that both parties can get their money back

# Transaction replacement using timelocks

channels which replace transactions using timelocks are known as 
duplex micropayment channels.

duplex micropayment channels
- replace transactions using timelocks
- subsequent commitment transactions use lower timelocks

ex:  the first commitment transaction is created with a timelock of 100 days.
, meaning it cannot be appended to the blockchain untill 100 days have passed.
The second commitment transaction is created with a timelock of 99 days
and spends the same funds, so it will be valid first.
the outdated commitment transactionn will never have a time where it can
be broadcast, as the referenced output will have been spent already.
Subseqent commitment transactions use lower timelocks

The commitment with the lowest timelock can be included in the blockchain
before the others

A channel constructed this way has to be closed by broadcasting the 
newest commitment transaction as soon as the first timelock has elapsed,
- limiting the maximum lifetime of a channel.

this problem can be solved elegantly with a kickoff transaction.
Timelock only start ticking as soon as the kickoff transaction is broadcast,
resulting in a potentially unlimited lifetime of a channel.
- you can't cash in you commitment transaction untill you also submit
the kickoff transaction.

a time locks count can be relative to the inclusion of a kickoff transaction.
no counter starts until the kickoff transaction has been broadcast.

as soon as the latest transactions timelock elapsed the 
channel has to be closed.
- this limits the lifetime of the channel

relative timelocks
- only when kickoff transaction is broadcast,
  then will the countdown start
  
a tree of transactions???

still one will quickly run out of time by doing transaction,
each requiring a smaller timelock.
- this was solved with a tree of transactions.
  + at any point in time only the path where all transactions
    have the lowest timelock of their siblings can be broadcast.
  + in this way, many commitment transactions can be created
    before the timelock get too low and the channel can not be updated any more

# transaction replacement using punishment
instead of on commitment transaction, two are created for each 
party with embeds a penaltiy if someone tries to submitt a previous
transaction.

revocable transactios to replace the commitments
- revealing a secret which allows oppenent to punish cheating.

# Channel factories

a new layer between the blockchain and the payment network,
giving a three-layered system

better to just say another off-chain protocol. 

this other off-chain protocol creates multi-party micropayment channels
we call channel factories, which can quickly fund regular two-party 
channels.

multi-party channels can be implemented with timelocks or punishments

timelocks scales better to larger participant numbers

the reqular micropayment channels can be punishment based


# hook transaction #
- the hook transaction is the funding transaction of a multi-party channel.
- it locks the funds of many parties into a shared ownership.

# allocation # 
the allocation is one transaction or a number of sequential transactions
that take the locked funds from a multi-party channel as an input 
and fund many mult-party channels with their outputs

the allocation effectively replaces the funding transaction of a number
of two-party channels.

# commitment #
a commitment is a transaction or a number of transaction that
return the funds of a two-party channel

a channel is constructed by first creating all transactions of the 
intial state,  then sign all except the hook.
and then signing and broadcasting the hook.

hooks can only be spent with the signatures of all parties

# 3.1 replaceable allocations ????

replaceable transaction with many parties can be implemented 
similarly to two-party channels based on timelocks with an 
invalidation tree and a kickoff transaction at the root

the leaves of the invalidation tree create the two-party shared accounts.



timelocks with an invalidation tree and a kickoff transaction at the root,
which starts the timmers when broadcast to the blockchain

the leaves of the invalidation tree create the two-party shared accounts.


# 3.2 settlement 

when the involved parties cooperatively decide to close a channel factory,
they can create and broadcast a settlement transaction.
which pays out the current stake of each party directly from the shared
account

only two transactions appear on the blockchain, the hook and the settlement

I one node decides to close the channel factory, it broadcast this
decision to all other nodes. everyone stop updating the subchannels and 
broadcasts the sum of his current stake.  this is enough information
for each node to create and sign the settlement transaction and broadcast 
the signature.
- lying does not help sine if any node gives a number to hight the
  total sum would exceed the locked-in funds and the settlement transacttion
  would be invalid

# 3.3 Moving funds

a channel factory can be use to rebalance channels that have become one sided.

a new allocation is setup which replaces every channel with a balanced new one.

- fund can also be moved between channels, 
- new channels can be created
- or old ones removed.

# 3.5 Leaving a group

there may be situations where some nodes wants to leave,
however the others would like to continue the channel

the allocation was replaced with a new hook.

after bordcasting the new hook, the leaving node
can spend its money on the blockchain and the others
can continue

# 3.6 Coordination of allocation updates

when a new allocation is created, the members of a channel factory
need to coordinate the creation of a new allocation transaction
and all transactions to make the new subchannels of this new allocation.

this could take a long time 

i. a member decides that an update of the allocation is necessary and
   broadcast to all nodes in the group that a new allocation should be
   created.
- e.g, it wants to move funds to another channel

ii.  as soon as someone receives the allocation update,
   he will issue a request to all his subchannel partners
   to use the current channel state as the base for new allocation

iii. the two cooperating parties decide on a starting state

iv.  each node creates the new allocation transaction.
     these should all be identical
	 
after receiving all signatures on the new allocation, a node can stop
updating the subchannels based on the old allocations.

normal channel operations can be continued as long as care is taken to make 
changes to the subchannels of both the old and the new allocation.

# 3.8 Risks

the number of parties that can stop cooperating and close the channel rises,
as anyone involved in the multi-party channel can broadcast the allocation
transaction to the blockchain.
- afterwards the subchannels can still be used,
  but the option to move funds between channels is lost.






